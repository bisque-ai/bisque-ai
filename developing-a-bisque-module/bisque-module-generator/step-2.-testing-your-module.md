# Step 2. Testing Your Module

## Testing Module

You can now start testing your module in Bisque. The first step is to edit your Dockerfile to create a new image that will include all the extra layers required for Bisque communication.

### **Editing Dockerfile**

It is necessary to append a few commands to your Dockerfile. These commands will install dependencies needed for python 3 modules, create the necessary folder structure, and copy the required files and folders into your module container. If your module is written in Python 3, you need to make sure to copy the `bqapi` folder from this repo in your `{ModuleName}` folder as described above.

Here is the updated Dockerfile for a simple Edge Detection module: _**Remember to change the .xml file name to your {ModuleName}.xml file**_

```
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

####################################################################
######################## Append From Here Down #####################
####################################################################

# ===============bqapi for python3 Dependencies=====================
# pip install in this exact order
RUN pip3 install six
RUN pip3 install lxml
RUN pip3 install requests==2.18.4
RUN pip3 install requests-toolbelt

# =====================Build Directory Structure====================

COPY PythonScriptWrapper.py /module/
COPY bqapi/ /module/bqapi

# Replace the following line with your {ModuleName}.xml
COPY EdgeDetection.xml /module/EdgeDetection.xml

ENV PATH /module:$PATH:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
ENV PYTHONPATH $PYTHONPATH:/module/src
```

### **Creating Module Image**

Once you update the Dockerfile, create a new Docker image by running the following command from your `{ModuleName}` folder. Note that docker images must have lowercase names and tags. The `biodev.ece.ucsb.edu:5000/` prefix is needed for Bisque to pull the image. Once you are ready to deploy your application to production, you will push this image to the specified repo.

```bash
docker build -t biodev.ece.ucsb.edu:5000/{modulename}:{tagname} .
```

It is good practice to specify a `{tagname}` that identifies the image versions. As you add functionalities and customize your module, it is a good idea to keep track of the various stable images of your module. By default, Docker will tag the image `latest` if a tag is not provided. The following are a few example of how you could tag your images:

```bash
# First stable module image
docker build -t biodev.ece.ucsb.edu:5000/edgedetection:v1.0.0 .
```

```bash
# Small bugs fixed
docker build -t biodev.ece.ucsb.edu:5000/edgedetection:v1.0.4 .
```

```bash
# Implementing some interactive parameters
docker build -t biodev.ece.ucsb.edu:5000/edgedetection:v1.2.0 .
```

```bash
# Second stable module image with all features implemented
docker build -t biodev.ece.ucsb.edu:5000/edgedetection:v2.0.0 .
```

You can also overwrite images by building with same `{modulename}:{tagname}` until you get to a point in which you want to keep that specific image as reference.

### **Updating runtime-module-cfg**

The runtime module configuration file specifies the image that Bisque will pull when a user runs your module.

The only line that should be updated is `docker.image = {modulename}:{tagname}`. This should specify the Docker image and tag name that you wish to test or deploy on Bisque. This file will be called when a user hits the **`Run`** button in your module's page on Bisque. Note that the prefix `biodev.ece.ucsb.edu:5000/` is not included in this line, Bisque will prepend it before pulling your local image.

{% hint style="info" %}
**IMPORTANT**

It is very important to update the `runtime-module.cfg` each time you build an image with a different name or tag so Bisque pulls the correct image you want to test.
{% endhint %}

***

This is an example of a `runtime-module.cfg` for the EdgeDetection module:

```bash
#  Module configuration file for local execution of modules

module_enabled = True
runtime.platforms = command

[command]
docker.image = edgedetection:v2.0.0     # Only edit this line
environments = Staged,Docker
executable = python PythonScriptWrapper.py
files = pydist, PythonScriptWrapper.py
```

### **Running Bisque Container**

Download the latest Bisque module development image by running:

```bash
docker pull amilworks/bisque-module-dev:git
```

The following command will launch a container named `bisque` on `http://{your.private.ip.address}:8080/`. The option `--v $(pwd):/source/modules` will mount your local module located in your current directory `$(pwd)` to your container at `/source/modules`. The `-v /var/run/docker.sock:/var/run/docker.sock` option will enable access to your local docker containers within the BisQue dev container. This is crucial because modules are ran as containers themselves so if you cannot run a container within a container, you will get an error.

```bash
# Navigate to the 'Modules' parent directory that holds your {ModuleName} folder.

ivan@bisque:~/Bisque$ cd ~/Bisque/Modules

# It should only contain the modules you intend to test on bisque, for Ex:

ivan@bisque:~/Bisque/Modules$ ls
EdgeDetection

# Run container
ivan@bisque:~/Bisque/Modules$ docker run --name bisque --rm -p 8080:8080 -v $(pwd):/source/modules -v /var/run/docker.sock:/var/run/docker.sock amilworks/bisque-module-dev:git
```

### **Logging Into Bisque**

Navigate to `http://{your.private.ip.address}:8080/` on any web browser and Bisque should be up and running. For example, if my ip address is `192.168.181.345`, you would navigate to `http://192.168.181.345:8080/`. You can find your private ip address by running:

```bash
ifconfig | grep "inet.*broadcast.*" | awk '{print $2}'
```

or

```bash
ifconfig | grep "inet.*Bcast.*" | awk '{print $2}'
```

It will be the address that has twelve digits with a period after every third digit. You can also read the following [article (Windows, Mac)](https://www.techbout.com/find-public-and-private-ip-address-44552/) or [article (Linux)](https://phoenixnap.com/kb/how-to-find-ip-address-linux) to find your private ip address if the previous commands didn't work.

If Bisque is not up, go into the container with `docker run -it amilworks/bisque-module-dev:git bash` and check the `bisque_8080.log` to debug. Report any issues on the [Bisque GitHub](https://github.com/UCSB-VRL/bisqueUCSB).

Login as admin using the credentials:

```bash
Username: admin
Password: admin
```

### **Upload Data**

You can now upload any data that you need to test your module. In the top menu bar, go to `Upload -> Choose files -> Upload (at the bottom)`. Exit out of the pop-up window and move on to register your module.

### **Register Module**

In the top right hand corner, go to `Bisque admin -> Module Manager`. In the right panel, `Engine Modules`, fill in the Engine URL with:

```
http://{your.private.ip.address}:8080/engine_service
```

You can find your private ip address as mentioned above.

Click `Load` to show all the modules that are in the `Modules` folder you mounted when running the Bisque docker image. Drag and drop the module you want to test from the right panel to the left and exit out of the setting window.

If you don't see any modules on this list, go throught the following debug process:

* Check that the `Engine URL` is correct. If your ip address is `192.168.181.345` for example, your engine url should be `http://192.168.181.345:8080/engine_service`.
* Make sure that the mounted folder `-v $(pwd):/source/modules` contains the `{ModuleName}` folders of the modules you want to test. You should be in your `Modules` folder when running the docker container.
* Make sure that the `.xml` file in your `{ModuleName}` folder and the `{ModuleName}` folder have the same name. For example, for the `EdgeDetection` module, {ModuleName} should be `EdgeDetection` and the `.xml` should be named `EdgeDetection.xml`.
* Make sure that your `{ModuleName}` folder has a `runtime-module.cfg` file.

### **Running Module**

To run your module, click the `Analyse` button from the top menu bar and choose your module. If you don't see your module, refresh the page and try again.
