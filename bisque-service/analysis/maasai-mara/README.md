# Maasai Mara

This section describes all modules developed for the Maasai Mara team.

## Objective

Primary objective of the project is to semi-automate the Maasai Mara's image processing pipelines using BisQue. In other words, the goal is to enable a Rear Seat Observer (at Mara Reserve) to upload the images to BisQue and semi-automate the image annotation and image analysis workflows by letting them leverage BisQue.

## BisQue Usage Overview

Users can come to BisQue with a dataset of images and use BisQue as follows:

<details>

<summary>Predict and Export</summary>

* Upload the dataset to BisQue

<!---->

* Run the AI model on the uploaded dataset

<!---->

* Update the Predictions, if required

<!---->

* Export the Annotations from BisQue

</details>

<details>

<summary>Annotate and Train</summary>

* Upload the dataset to BisQue

<!---->

* Hand Annotate the Dataset or Import the Annotations to BisQue

<!---->

* Train the AI model

</details>

<details>

<summary>Report Generation and Evaluations</summary>

* Upload the dataset to BisQue

<!---->

* Run the AI model on the uploaded dataset

<!---->

* Generate Reports on Animal Counts

<!---->

* Compare the model predictions with ground truth (if exists)

<!---->

* Build Dashboards using BisQue API

</details>



In other words, Users can just login to BisQue from a browser and do the following:

<figure><img src="../../../.gitbook/assets/Copy of three_verticals (2).jpg" alt=""><figcaption><p>BisQue usage - Anticipated Flow chart</p></figcaption></figure>



## Developed Modules

The above mentioned flow can be achieved using the modules described below:

* Module to Tag Annotations from a text file

{% content-ref url="mara-animal-annotator.md" %}
[mara-animal-annotator.md](mara-animal-annotator.md)
{% endcontent-ref %}

* Module to Detect Animals using AI

{% content-ref url="mara-animal-detector.md" %}
[mara-animal-detector.md](mara-animal-detector.md)
{% endcontent-ref %}

* Module to Evaluate a machine learning model

{% content-ref url="mara-evaluator.md" %}
[mara-evaluator.md](mara-evaluator.md)
{% endcontent-ref %}

* Module to generate report on a given dataset

{% content-ref url="mara-report-generator.md" %}
[mara-report-generator.md](mara-report-generator.md)
{% endcontent-ref %}

* Module to Retrain the AI Model

{% content-ref url="train-a-new-detector.md" %}
[train-a-new-detector.md](train-a-new-detector.md)
{% endcontent-ref %}





