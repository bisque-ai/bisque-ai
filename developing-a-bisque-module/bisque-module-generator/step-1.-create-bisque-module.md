# Step 1. Create BisQue Module

## Generating Module Files

Now that you have containerized and tested your application, you are ready to pull this repo and create the Bisque module.

### **Installing the bqmod CLI**

First step is to clone this repo and install the CLI. You can create a virtual environment in which to install the CLI if you wish.

```bash
# Go to a directory outside your Modules folder and clone the rep
ivan@bisque:~/Bisque/Modules$ cd ~/Bisque
ivan@bisque:~/Bisque$ git clone https://github.com/ivanfarevalo/BQ_module_generator.git

# Go into the BQ_module_generator folder
ivan@bisque:~/Bisque$ cd BQ_module_generator

# This is optional
ivan@bisque:~/Bisque/BQ_module_generator$ virtualenv bqvenv
ivan@bisque:~/Bisque/BQ_module_generator$ . bqvenv/bin/activate

# This is required
(bqvenv) ivan@bisque:~/Bisque/BQ_module_generator$ pip3 install --editable .
```

Test the installation by running `bqmod --help`. You should get a help message. Remember to activate your virtual environment if you used one during installation, otherwise you wont be able to run the commands.

### **Using the bqmod CLI**

_**For Python3 modules, you must copy the bqapi folder from the `BQ_module_generator` folder into your `{ModuleName}` folder.**_ Your folder structure should look like this so far:

```
-- Modules
    -- {ModuleName}
        -- bqapi (Only for python3 modules)
        -- Dockerfile
        -- src
            -- {source_code}
            -- BQ_run_module.py
```

The **bqmod** CLI uses simple commands to populate a .json file with the configurations details of your module. All commands must be ran in your `{ModuleName}` folder and are preceded with the `bqmod` command.

| Command                   | Options          | Description                                                                                                                                                               |
| ------------------------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`bqmod`**               | --help           | Shows help information on how to use the CLI                                                                                                                              |
| **`bqmod init`**          |                  | Initializes configuration file for your module and pulls necessary files from repo. If one already exists, it wills ask whether you would like to overwrite it.           |
| **`bqmod set`**           | -n --name        | Sets or changes the {ModuleName} field. This must match the {ModuleName} of your module folder and should not have spaces. Ex: **`bqmod set -n "EdgeDetection"`**         |
|                           | -a --authors     | Sets or changes the name of the authors. Ex: **`bqmod set -a "Ivan"`**                                                                                                    |
|                           | -d --description | Sets or changes a short description of the module. Must be in quotations. Ex: **`bqmod set -d "This module finds edges in images"`**                                      |
| **`bqmod inputs`**        | --remove         | Flag to remove input specified by --name flag                                                                                                                             |
|                           | -i --image       | Flag that sets an input of type image.                                                                                                                                    |
|                           | -t --table       | Flag that sets an input of type table.                                                                                                                                    |
|                           | -f --file        | Flag that sets an input of type file.                                                                                                                                     |
|                           | -n --name        | Required parameter. Sets the name of the input as will be shown in Bisque module page. _**Input names MUST match input\_path\_dict keys in BQ\_module\_run.py.**_         |
| **`bqmod outputs`**       | --remove         | Flag to remove output specified by --name flag.                                                                                                                           |
|                           | -i --image       | Flag that sets and output of type image.                                                                                                                                  |
|                           | -t --table       | Flag that sets and output of type table.                                                                                                                                  |
|                           | -f --file        | Flag that sets and output of type file.                                                                                                                                   |
|                           | -n --name        | Required parameter. Sets the name of the output as will be shown in Bisque results section. _**Output names MUST match output\_paths\_dict keys in BQ\_module\_run.py.**_ |
| **`bqmod summary`**       |                  | Prints out the current module configurations.                                                                                                                             |
| **`bqmod gen_help_html`** |                  | Generates help.html from help.md.                                                                                                                                         |
| **`bqmod create_module`** |                  | Generates the module .xml.                                                                                                                                                |

{% hint style="warning" %}
**Names for Inputs and Outputs **_**MUST**_** Match**

_It is crucial to note that the names for inputs and outputs MUST match the dictionary keys of `input_path_dict` and `output_paths_dict` respectively!_ Failure to ensure this will result in an error at runtime.
{% endhint %}

![Dictionary Keys Example](https://github.com/ivanfarevalo/BQ\_module\_generator/raw/main/public/dict\_name\_ex.png)

Here's an example of creating a simple Edge Detection module:

```bash
ivan@bisque:~/Bisque/Modules$ cd EdgeDetection
ivan@bisque:~/Bisque/Modules/EdgeDetection$ bqmod init
ivan@bisque:~/Bisque/Modules/EdgeDetection$ bqmod set -n "EdgeDetection"
ivan@bisque:~/Bisque/Modules/EdgeDetection$ bqmod set -a "Ivan"
ivan@bisque:~/Bisque/Modules/EdgeDetection$ bqmod set -d "This module finds edges in images"
ivan@bisque:~/Bisque/Modules/EdgeDetection$ bqmod inputs --image -n "Input Image"   # THIS MUST MATCH DICTIONARY KEYS in input_path_dict
ivan@bisque:~/Bisque/Modules/EdgeDetection$ bqmod outputs --image -n "Output Image"   # THIS MUST MATCH DICTIONARY KEYS in output_paths_dict
ivan@bisque:~/Bisque/Modules/EdgeDetection$ bqmod summary
Name: EdgeDetection
Author: Ivan
Description: This module finds edges in images
Inputs: {'Input Image': 'image'}
Outputs: {'Output Image': 'image'}
ivan@bisque:~/Bisque/Modules/EdgeDetection bqmod create_module
EdgeDetection.xml created
```

### **Generating help html file**

Edit the `help.md` [markdown](https://www.markdownguide.org/basic-syntax/) file in the `public` folder to include any documentation and examples you want to provide users. When you are done, generate the html file by running `bqmod gen_help_html` from the `{ModuleName}` folder. Please do a hard refresh on your browser every time you make changes to `help.html`. If not, it's possible that the browser is just showing the old version of the file because it was cached.

**Module Folder Structure**

This should be the resulting folder structure after creating the module.

```
-- Modules
    -- {ModuleName}
        -- bqapi (Only for python3 modules)
        -- bqconfig.json
        -- public
            -- thumbnail.png (module icon for bisque)
            -- help.md (Help markdown)
            -- help.html (Help html)
        -- Dockerfile
        -- PythonScriptWrapper.py
        -- runtime-module.cfg
        -- src
            -- {source_code}
            -- BQ_run_module.py
        -- {ModuleName}.xml
        -- xml_template.xml
```
