---
description: Module to evaluate the inference with respect to a ground truth
---

# Mara Evaluator

This page describes how one can run the BisQue Module named `MaraEvaluation`

## Run Instructions

This module takes in a dataset of images along with two valid G-Object names and generates a confusion matrix that represents the similarity or dissimilarity between ground truth annotations and predicted annotations.

### Navigate to Module Page

[Login](../../login-signup.md) >> Analyze >> Masai Mara (in Groups Column) >> `MaraEvaluation`

### Expected Inputs

* An image or a dataset of images
  * [Click here](https://bisque2.ece.ucsb.edu/client\_service/view?resource=https://bisque2.ece.ucsb.edu/data\_service/00-KiEfGPpfTrHigpoUtTjKgB) for a sample dataset of input images
* Ground Truth Annotations
  * String representing the name of a G-Object (Ex: `ground_truth_annotations`)
* Predicted Annotations
  * String representing the name of a G-Object (Ex: `annos_from_AI`)

<figure><img src="../../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

### Expected Outputs

* High level stats along with confusion matrix
  * A Table that can be exported as CSV
  * A Consfusion Matrix

<figure><img src="../../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

Better Visualization of Confusion Matrix:

![](<../../../.gitbook/assets/image (2).png>)
