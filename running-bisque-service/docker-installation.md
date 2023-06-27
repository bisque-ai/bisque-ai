---
description: Install BisQue locally using Docker
---

# Docker Installation

## **Using BisQue with Docker**

Ensure you have the latest release by first running the following pull command:

```
  docker pull amilworks/bisque-module-dev:git
```

### **Run the Container**

To run the docker version of BisQue locally, start a bisque server on the host port 8080:

```
docker run --name bisque --rm -p 8080:8080 amilworks/bisque-module-dev:git
```

and point your browser at `http://localhost:8080`. You should see a BisQue homepage similar to the one on bisque2.ece.ucsb.edu. If you do **not** see the homepage, check to make sure that port 8080 is not being used by another container or application _and_ that you have correctly mapped the ports using `-p 8080:8080`, where `-p` is short for port.

### **Registering Modules**

To register all the modules to your local server:

* Login to your BisQue server using admin:admin
* Find the module manager under the Admin button on the top right hand corner
* Put `http://localhost:8080/engine_service` in the right panel where it says **Enter Engine URL** and hit Load
  * NOTE: Use `localhost:8080` here because it's internal to the container.
* Drag and Drop **MetaData** to the left panel---or whatever module you would like---and the module will now be registered and available for use. You can make the module Public by hitting **Set Public** in the left panel, which basically means the module is Published and ready for public use.

### **Custom Modules, Copying Folders out of the Container**

If you would like to build and test your own module locally, using host mounted modules will make life easier to build, test, debug, and deploy locally.

1.  Copy the module directory out of the container and into the folder on your local system named `container-modules`.

    ```
       docker cp bisque:/source/modules container-modules
    ```
2. Copy your module into the `container-modules` folder on your local system.
3.  Restart the container with host mounted modules. Be careful with the command `$(pwd)/container-modules` that we are using here. If the `/container-modules` is **not** in the specified path, you will not see any of the modules during the registration process.

    ```
    docker stop bisque
    docker run --name bisque --rm -p 8080:8080 -v $(pwd)/container-modules:/source/modules  amilworks/bisque-module-dev:git
    ```
4. Register your module from the above steps in [Registering Modules](docker-installation.md#registering-modules), using the Module Manager.

> **Pushing your Module to Production.** If you feel that your module is ready to be added to the production version of BisQue, please feel free to contact us and we will gladly begin the process.

### **Data Storage**

Use an external data directory so you don't lose data when the service stops - Uploaded image and workdirs are store in `/source/data`. You can change this to be a host mounted directory with

```
docker run --name bisque --rm -p 8080:8080 -v $(pwd)/container-data:/source/data  amilworks/bisque-module-dev:git
```

* The default sqlite database is stored inside the container at `/source/data/bisque.db`
* The uploaded images are stored inside the container at `/source/data/imagedir`

### **View Downloaded Images, Running Containers**

List all the docker **images** on your system:

```
docker images
```

List all running **containers** on your system:

```
docker ps
```

### **SSH into the Container**

If you would like to see everything inside the container, you can use the following command _while the container is running_:

```
docker exec -it amilworks/bisque-module-dev:git bash
```

The `-it` flag enables you to run interactively inside the container. There are numerous other flags you can take advantage of as shown here:

```
--detach ,      -d    Detached mode: run command in the background
--detach-keys         Override the key sequence for detaching a container
--env ,         -e    Set environment variables
--interactive , -i    Keep STDIN open even if not attached
--privileged          Give extended privileges to the command
--tty ,         -t    Allocate a pseudo-TTY
--user ,        -u    Username or UID (format: <name|uid>[:<group|gid>])
--workdir ,     -w    Working directory inside the container
```

Now, let's say you want to ssh into an image without fully starting BisQue. More precisely, you want to ssh into a non-running container. You can accomplish this by running:

```
docker run -it amilworks/bisque-module-dev:git bash
```

If you want to **exit**, simply type `exit` and you will be taken back outside of the container.

### **Stop the Container**

Say you are done playing with your container for today, you can stop the container by using the following command:

```
docker stop amilworks/bisque-module-dev:git 
docker stop {YOUR_CONTAINER_NAME} #  <--- If you named the container
```
