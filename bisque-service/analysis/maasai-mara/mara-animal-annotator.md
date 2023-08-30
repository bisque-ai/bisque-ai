---
description: Module to tag annotations to a bisque dataset
---

# Mara Animal Annotator

This page describes how one can run the BisQue Module named `Mara Animal Annotator`

## Run Instructions

This module takes in an image or a dataset of images, along with a text file containing the annotations that correspond to input dataset. It tags the bounding boxes in the text file to input images as graphical objects (commonly referred to as `G-Objects` in BisQue).

### Navigate to Module Page

[Login](../../login-signup.md) >> Analyze >> Maasai Mara (in Groups Column) >> `Mara Animal Annotator`

### Expected Inputs

* An image or a dataset of images
  * [Click here](https://bisque2.ece.ucsb.edu/client\_service/view?resource=https://bisque2.ece.ucsb.edu/data\_service/00-KiEfGPpfTrHigpoUtTjKgB) for a sample dataset of input images
* Annotation File
  * [Click here](https://bisque2.ece.ucsb.edu/client\_service/view?resource=https://bisque2.ece.ucsb.edu/data\_service/00-aZxfYuTBhne4uQSUv5Mkd) for a sample model file
* Annotation Name
  * This can be any string that can be used to name the annotations that are going to be tagged as inputs (for example, `ground_truth_annos`).

<figure><img src="../../../.gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>

### Expected Outputs

* Annotations added to the image as G-Objects

Once the module execution is completed, one can navigate to the input dataset to visualize the tagged annotations.

<figure><img src="../../../.gitbook/assets/temp.jpg" alt=""><figcaption></figcaption></figure>
