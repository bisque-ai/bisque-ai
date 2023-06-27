---
description: Use the BQAPI to upload and download data from BisQue
---

# Install Python BQAPI

## PyPi Install

```shell
pip install bqapi-ucsb
```

## Tutorials

We have two tutorials to get you started with using the BQAPI.&#x20;

{% content-ref url="download-an-array.md" %}
[download-an-array.md](download-an-array.md)
{% endcontent-ref %}

{% content-ref url="upload-an-image.md" %}
[upload-an-image.md](upload-an-image.md)
{% endcontent-ref %}

## BQAPI&#x20;

BQAPI provides bisque users with a means to extract features from resources using the feature service. Below is information on the Python API and a few example on how to use it.

```python
FeatureResource(image=None, mask=None, gobject=None)
```

Named tuple to make it easier to organize resources. Of course, one can always just make a simple list of tuples for the resource list.

> `class Feature()`

The Feature class is the base implementation of the feature service API for when one need to extract a set of features on a small set of resources. BQSession is used as the communication layer for bqfeatures, therefore before making any requests a local BQSession has to be instantiated to pass to any feature request.

```python
fetch(session, name, resource_list, path=None)
```

Requests the feature server to calculate features on provided resources.

**Input**

* session - a local instantiated BQSession attached to a MEX.
* name - the name of the feature one wishes to extract.
* resource\_list - list of the resources to extract. Format: \[(`image_url`, `mask_url`, `gobject_url`),...] if a parameter is not required just provided None.
* path - the location were the HDF5 file is stored. If None is set, the file will be placed in a temporary file and a Pytables file handle will be returned. (default: None)

**Output**

* Returns either a Pytables file handle or the file name when the path is provided

Lets upload an image an calculate a single feature on it.

```python
import os
from bqapi.comm import BQSession
from bqapi.bqfeature import Feature, FeatureResource

# initialize local session
session = BQSession().init_local(user, pwd, bisque_root='http://bisque.ece.ucsb.edu') 

#post image to bisque and get the response
response_xml = session.postblob('myimage.jpg') 

#construct resource list of the image just uploaded
resource_list = [FeatureResource(image='http://bisque.ece.ucsb.edu/image_service/%s' % response_xml.attrib['resource_uniq'])]

#fetch features on image resource
pytables_response = Feature().fetch(session, 'HTD', resource_list)

#get a numpy list of features for the downloaded HDF5 file.
feature = pytables_response.root.values[:]['feature']

#close and remove the HDF5 file since it is stored in a tempfile
pytables_response.close()
os.remove(pytables_response.filename)
```

> `fetch_vector(session, name, resource_list)`

Requests the feature server to calculate features on provided resources. Designed more for requests of very few features since all the features will be loaded into memory.

**Input**

* session - a local instantiated BQSession attached to a MEX.
* name - the name of the feature one wishes to extract
* resource\_list - list of the resources to extract. format: \[(image\_url, mask\_url, gobject\_url),...] if a parameter is not required just provided None

**Output**

* return - a list of features as numpy array

> `length(session, name)`

Static method that returns the length of the feature requested.

**Input**

* session - a local instantiated BQSession attached to a MEX.
* name - the name of the feature one wishes to extract

**Output**

* return feature length

### ****

### ****
