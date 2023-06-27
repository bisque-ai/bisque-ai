# Download an Array

## Access an HDF5 Table from BisQue

In this example, we will show you how to use the BQAPI to download a multidimensional array from within an HDF file and return it as a `numpy` array in `Python`.

***

The API call goes as follows:

1. Get the logger
2. Instantiate a BisQue session
3. Login using `USERNAME` and `PASSWORD`
4. Instantiate the `table` service
5. Use the table service to load the array
6. Return the array

***

### **Step 0. Import Dependencies**

Before we can even attempt anything cool with the API, we need to import the necessary packages. In this case, we need the following packages:

```python
import numpy as np
import logging

from bqapi.services import tables
from bqapi import BQSession
from bqapi.util import fetch_blob
from bqapi.comm import BQCommError
from bqapi.comm import BQSession
```

Place these at the top of your Jupyter notebook or `Python` script to ensure these run first. If you have not installed the BQAPI via pip, then install the BQAPI here.

### **STEP 1. Get the Logger**

The logger service allows us to debug if anything goes wrong during the process of pulling our array. We use this in the BisQue core development as well, so feel free to gain greater insight into our other logger services as well.

We will define the `get_table` logger here.

```python
log = logging.getLogger('get_table')
```

### **STEP 2. Instantiate a BisQue session**

This instantiation enables the user to effectively communicate with BisQue. Without this, you will not be able to login and interact with the API.

```python
bq = BQSession()
```

### **STEP 3. Login using BisQue Credentials**

Here is where we will login into our BisQue account to access the data we have uploaded. We show an alternative chained version (line 2) of the commands here to instantiate the BQSession and login at the same time.

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

### **STEP 4. Instantiate the `table` service**

Now we need to instantiate the table service. To do this, use _service_ to call the table service. Simple, right?

```python
table_service = bq.service ('table')
```

### **STEP 5. Using the Table Service**

In this example, we use the `load_array` function from the table service to return a `numpy` array from the respective HDF file on BisQue. What is most important is the input to this function, which is as follows:

```python
data_array = table_service.load_array(table_uniq, path, slices=[])
```

**Inputs**

* `table_uniq:` BisQue URI (**Required**)
* `path :` Path to table within HDF (**Required**)
* `slices :` Slice of an array (**Optional**)

**What is the `table_uniq`**

The `table_uniq` argument comes from how BisQue handles resources. Let's say you upload an image of a cat. BisQue will automatically assign a unique ID or, **URI**, to that image. Here is an example image:

```
https://bisque.ece.ucsb.edu/client_service/view?resource=https://bisque.ece.ucsb.edu/data_service/00-s5b358UmuziTaUqqYtTcPF
```

The last portion of the url is the URI. This is what you need to use as the input to the `table_uniq` argument.

```
https://bisque.ece.ucsb.edu/data_service/00-s5b358UmuziTaUqqYtTcPF
                                         ^-----------------------^
                                              COPY TABLE URI
```

**What is the `path`**

Say we have an array stored in a specific path in our HDF file. We can define a variable named `table_path` and place that after the `table_uniq` argument.

**Example**

```python
table_path = 'PATH/TO/ARRAY/IN/HDF'
table_service.load_array(uri, table_path)
```

**Example. Functionalizing the Boring Stuff: `get_table`**

Here we provide a full working example of how to functionalize the entire process. Overall, the structure is the same as the sum of its pieces, but now we can import many arrays into, say, our Jupyter Notebook for simple data processing tasks. You can also extend this example to upload the table back to BisQue once the data preprocessing is done.

```python
def get_table(user,pwd, table_PATH, uri=None,root='https://bisque.ece.ucsb.edu'):
    
    log = logging.getLogger('get_table')
    print("Starting Session...")
    
    bq = BQSession().init_local(user, pwd, bisque_root=root)
    print("BQSession Established. Attempting to fetch data...")
    
    table_service = bq.service ('table')
    print("Successfully Retrieved Array!")
    logging.basicConfig()

    
    return table_service.load_array(uri, table_PATH)
```

**Let's run our function!**

In this example, we are using the BQAPI to access an HDF file that contains a two-phase 3D microstructure.

```python
ms = get_table(amil_khan, bisque1234, 
              'DataContainers/ImageDataContainer/CellData/Phases',
              '00-orJQLiXgqh8K955C6qzhyC')
print('\nShape of table: {}'.format(ms.shape))
```

**`Output:`**

```
Starting Session...
BQSession Established. Attempting to fetch data...
Successfully Retrieved Array!

Shape of table: (5, 5, 5, 1)
```
