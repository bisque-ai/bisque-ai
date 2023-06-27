# Step 0. Organizing Your Code

## Before Cloning the GitHub Repo

Before cloning this repo, structure your module in the following manner.

```
-- Modules
    -- {ModuleName}
        -- src
            -- {source_code}
            -- BQ_run_module.py
```

You should create a `Modules` folder which will only contain the modules that you wish to test in Bisque. You should name your `{ModuleName}` folder how you would like your module to appear in bisque, for ex. `EdgeDetection`. Create a folder named `src` and place all your source code inside it. Finally, create a python file named `BQ_run_module.py` inside the `src` folder.

### **`BQ_run_module.py`**

Include all necessary data reading and pre-processing code in `BQ_run_module.py` as well as a function named `run_module` that will take `input_path_dict` and `output_folder_path`. Hyper parameters for running the module will have to be hardcoded for now but future releases will extend functionality for these as well. This function should load input resources from `input_path_dict`, do any preprocessing steps, run the algorithm, save all outputs to `output_folder_path`, _**AND return the `outputs_path_dict`**_.

### **`input_path_dict`**

The `input_path_dict` parameter is a dictionary with input names as keys and their corresponding paths as values.&#x20;

{% hint style="info" %}
**NOTE**

_It is important to note that these input names will be the labels that Bisque will display in your module web page._
{% endhint %}

_****_

![Input Example](https://github.com/ivanfarevalo/BQ\_module\_generator/raw/main/public/input\_ex.png)

_**These input names must also match the input names specified with the cli in a later step.**_ This will become clear later, for now, just choose some descriptive and unique input names that you would like Bisque to display in the module web page. You will use these input names to index the `input_path_dict` dictionary and load each resource from its respective path. Ex:

```python
input_img_path = input_path_dict['Input Image']
img = cv2.imread(input_img_path, 0)
```

### **`output_folder_path`**

The `output_folder_path` parameter is a path to the directory where output results should be saved.

### **`output_paths_dict`**

You should save all output result paths into a dictionary with descriptive and unique output names as keys. _**These output names will be used by Bisque as labels in your module results web page.**_&#x20;

**Example**

```python
output_paths_dict = {}
output_paths_dict['Output Image'] = output_img_path
```

![Output Example](https://github.com/ivanfarevalo/BQ\_module\_generator/raw/main/public/output\_ex.png)

These output paths must also be the same names used to specify outputs with the cli at a later step. The `run_module` function must return this dictionary of output paths in order for Bisque to read and post results back to the module web page.

A sample `BQ_run_module.py` is shown below:

```python
import cv2
import os
from detection import canny_detector

# input_path_dict will have input file paths with keys corresponding to the input names set in the cli.
def run_module(input_path_dict, output_folder_path, min_hysteresis=100, max_hysteresis=200):
    """
    This function should load input resources from input_path_dict, do any pre-processing steps, run the algorithm,
    save all outputs to output_folder_path, AND return the outputs_path_dict.
    
    :param input_path_dict: Dictionary of input resource paths indexed by input names. 
    :param output_folder_path: Directory where to save output results.
    :param min_hysteresis: Tunable parameter must have default values.
    :param max_hysteresis: Tunable parameter  must have default values.
    :return: Dictionary of output result paths.
    """
    
    ##### Preprocessing #####

    # Get input file paths from dictionary
    input_img_path = input_path_dict['Input Image'] # KEY MUST BE DESCRIPTIVE, UNIQUE, AND MATCH INPUT NAME SET IN CLI

    # Load data
    img = cv2.imread(input_img_path, 0)

    ##### Run algorithm #####

    edges_detected = canny_detector(img, min_hysteresis, max_hysteresis)


    ##### Save output #####

    # Get filename
    input_img_name = os.path.split(input_img_path)[-1][:-4]

    # Generate desired output file names and paths
    output_img_path = "%s/%s_out.jpg" % (output_folder_path, input_img_name) # CHECK FILE EXTENSION!

    # Save output files
    cv2.imwrite(output_img_path, edges_detected)

    # Create dictionary of output paths
    output_paths_dict = {}
    output_paths_dict['Output Image'] = output_img_path  # KEY MUST BE DESCRIPTIVE, UNIQUE, AND MATCH OUTPUT NAME SET IN CLI

    ##### Return output paths dictionary #####  -> IMPORTANT STEP
    return output_paths_dict

if __name__ == '__main__':
    # Place some code to test implementation
    
    # Define input_path_dict and output_folder_path
    input_path_dict = {}
    current_directory = os.getcwd()
    # Place test image in current directory
    input_path_dict['Input Image'] = os.path.join(current_directory,'test_image.jpg') # KEY MUST MATCH INPUT NAME SET IN CLI
    output_folder_path = current_directory
    
    # Run algorithm and return output_paths_dict
    output_paths_dict = run_module(input_path_dict, output_folder_path, min_hysteresis=100, max_hysteresis=200)
    
    # Get outPUT file path from dictionary
    output_img_path = output_paths_dict['Output Image'] # KEY MUST MATCH OUTPUT NAME SET IN CLI
    # Load data
    out_img = cv2.imread(output_img_path, 0)
    # Display output image and ensure correct output
    cv2.imshow("Results",out_img)
```

{% hint style="danger" %}
**🚨 ATTENTION**

_IT IS IMPORTANT TO **TRIPLE CHECK OUTPUT FILE EXTENSIONS** TO AVOID BUGS WHEN UPLOADING RESULTS BACK TO BISQUE!_
{% endhint %}

### **Containerizing application**

Test your `BQ_run_module.py` file by writing some test code in the `if __name__ == '__main__':` code block. A simple test implementation is shown above. Once `BQ_run_module.py` is working as expected, you can containerize your application with docker. Follow the instructions on [downloading docker](https://www.docker.com/products/docker-desktop), [creating a Dockerfile](https://docker-curriculum.com/#dockerfile), and [running a container](https://docker-curriculum.com/#docker-run). Here is an example of a Dockerfile for a simple edge detection module.

_**You must include the section**** ****`Copy Source Code`**** ****in your own Dockerfile.**_

```docker
# ==================================================================
# module list
# ------------------------------------------------------------------
# python        3.6    (apt)
# ==================================================================

FROM python:3.6.15-buster

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update
RUN apt-get install ffmpeg libsm6 libxext6 -y

# ===================Module Dependencies============================

RUN pip3 install cycler imageio kiwisolver matplotlib numpy opencv-python Pillow pyparsing python-dateutil scipy 

# ===================Copy Source Code===============================

RUN mkdir /module
WORKDIR /module

COPY src /module/src
```

In your `{ModuleName}` folder, you can create your image by running&#x20;

{% code title="Terminal" %}
```shell
docker build -t {modulemame}:v0.0.0 .
```
{% endcode %}

**Note** that docker images are only allowed to have lowercase letters.&#x20;

Run your docker container with `docker run -it {modulemame}:v0.0.0 bash` and test your application inside the container by calling `python BQ_run_module.py`.
