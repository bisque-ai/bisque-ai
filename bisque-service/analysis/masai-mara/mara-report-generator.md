---
description: Module to generate a report with respect to G-Object
---

# Mara Report Generator

This page describes how one can run the BisQue Module named `ReportGeneration`

## Run Instructions

This module takes in an image or a dataset of images, along with a name of a valid G-Object and generates CSV files and few visuals to illustrate the high level statistics on the animals present in the dataset and that are tagged to input G-object.

### Navigate to Module Page

[Login](../../login-signup.md) >> Analyze >> Masai Mara (in Groups Column) >> `ReportGeneration`

### Expected Inputs

* An image or a dataset of images
  * [Click here](https://bisque2.ece.ucsb.edu/client\_service/view?resource=https://bisque2.ece.ucsb.edu/data\_service/00-wzri2GdPGYauPHxA2KimU6) for a sample dataset of input images
* Annos to Analyse
  * String representing the name of the G-Object (Ex: `annos_from_AI`)

<figure><img src="../../../.gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

### Expected Outputs

* High level stats corresponding to the Dataset and G-Object.
  * A Table that can be exported as CSV
  * A pie chart representing the class distribution

<figure><img src="../../../.gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>
