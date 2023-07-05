---
description: How to upload an image to UCSB BisQue Production instance using the BQAPI
---

# Upload an Image

## **Uploading an Image to BisQue**

In this example, we will show you how to use the BQAPI to upload one of the over 100+ supported Biological formats from a Jupyter Notebook.



The API call goes as follows:

1. Instantiate a BisQue session
2. Login using `USERNAME` and `PASSWORD`
3. Upload the Image



### **STEP 0. Import Dependencies**

Before we can even attempt anything cool with the API, we need to import the necessary packages. In this case, we need the following packages:

```python
import os
from bqapi.comm import BQSession
```

Place these at the top of your Jupyter notebook or `Python` script to ensure these run first. If you have not installed the BQAPI via pip, then install the BQAPI here.

### **STEP 1. Instantiate a BisQue session**

This instantiation enables the user to effectively communicate with BisQue. Without this, you will not be able to login and interact with the API.

```python
bq = BQSession()
```

### **STEP 2. Login using BisQue Credentials**

Here is where we will login into our BisQue account to upload the data to our account. We show an alternative chained version (line 2) of the commands here to instantiate the BQSession and login at the same time.

```python
bq.init_local(user, pwd, bisque_root=root)
# bq = BQSession().init_local(user, pwd, bisque_root=root)
```

**Inputs**

If you do not have an account on BisQue, [make an account here](https://bisque.ece.ucsb.edu/registration/new).

* `USER` BisQue Username
* `PASSWORD` BisQue Password
* `bisque_root` "https://bisque.ece.ucsb.edu"

**Example.**

```python
bq.init_local(user=amil_khan, pass=bisque1234,
              bisque_root="https://bisque.ece.ucsb.edu")
```

### **STEP 3. Upload the Image**

The final step is to upload the image on your local system to BisQue. To do this, we will use the instantiated session `bq` along with the `postblob` function to upload the `NIFTI` image below.

```python
# Post image to BisQue and get the response
image_path = '/PATH/TO/IMAGE/supercoolscientificimage.DICOM'
img_upload = bq.postblob(image_path) 
```

**Example.** If your image is in the current directory of your Jupyter Notebook, then simply input the filename. Otherwise, specify the full path `/home/amil/Documents/T1_brain.nii.gz` or `~/Documents/T1_brain.nii.gz`.

```python
img_upload = bq.postblob('T1_brain.nii.gz') 
```
