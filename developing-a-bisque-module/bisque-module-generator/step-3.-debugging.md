# Step 3. Debugging

## Debug Steps

Follow the steps to run your module and verify that the result is as expected. If Bisque reports any errors or seems to be frozen, you can debug it by checking the logs in the Bisque container.

### **Log files**

Every time you hit the `Run` button, Bisque starts a new container of your module and saves their corresponding logs in `/source/staging/{mex_id}`. Mex id's start with `00-` followed by 22 alphanumeric characters, Ex: `00-fMqFvjiHRjUfaff6GRy73M`. This folder will have all the logs needed to debug your module. To get to this folder, run the following:

```bash
docker exec -it amilworks/bisque-module-dev:git bash
cd /source/staging/{mex_id}
```

The **`docker_run.log`** logs information regarding the pulling, starting, and stopping of your module container. The `PythonScript.log` logs information regarding the communication between Bisque and your module. As mentioned at the beginning of these guide, if you would like to add custom outputs or interactive parameters, follow the in depth [guide](https://ucsb-vrl.github.io/bisqueUCSB/module-development.html) on creating modules.
