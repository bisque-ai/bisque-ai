---
description: >-
  This module takes an underwater video from DUSIA dataset as the input and
  outputs a labeled video, counts, and annotations.
---

# Underwater Species Tracker and Counter

This page describes how one can run the BisQue Module named [`SpeciesTrackerAndCounter`](https://bisque2.ece.ucsb.edu/module\_service/SpeciesTrackerAndCounter/?wpublic=1)

## Run Instructions

This module takes in a video and generates outputs a labeled video, counts, and annotations for YOLO and CVAT.

### Navigate to Module Page

[Login](../login-signup.md) >> Analyze >> SpeciesTrackerAndCounter

### Expected Inputs

* DUSIA under water videos (e.g. [_example.mp4_](https://bisque2.ece.ucsb.edu/client\_service/view?resource=https://bisque2.ece.ucsb.edu/data\_service/00-FM84WchNQhjwAGaTaNvvBL))

<figure><img src="../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

### Expected Outputs

* Videos with detection, track, and counts labels (_example\_output.mp4_)
* Species counts table (_example\_count.hdf5_)
* YOLO Annotation file (_YOLO\_example\_annotations.txt_)
* CVAT Annotation file (_CVAT\_example\_annotations.xml_)

<figure><img src="../../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

### Instructions on Using The Module

<figure><img src="../../.gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

### Uploading Output Annotation files to CVAT

1. Download CVAT Annotation File from the module outputs

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

3. Open CVAT and upload the same video
4. In the Actions drop-down menu, select `Upload annotations` (Please make sure to delete any existing annotations on CVAT)

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

5. Choose CVAT 1.1 and drag the downloaded CVAT annotation file

<img src="../../.gitbook/assets/image (2).png" alt="" data-size="original">

6. Now visualize the predictions and fix them

<figure><img src="../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>
