---
description: Data upload from a web browser like Chrome
---

# Data Upload from Web

## Uploading Data to BisQue Service

### Step 1. Login

Make sure you are [logged in](../login-signup.md). If you are logged in, the MENU bar will include the **Upload** option.

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

### Step 2. Upload File or Folder

Click **Upload** from the toolbar. There will be two options: **Choose File** or **Choose Directory**.

<figure><img src="../../.gitbook/assets/folderupload.png" alt=""><figcaption></figcaption></figure>

### Step 3. Build a Dataset

Selecting **Choose Directory** will upload _all_ files in that directory. For example, we selected a folder with three NIFTI files.

Once the upload is finished, there will be two options to create a new dataset or add to an existing dataset.

{% tabs %}
{% tab title="Add to New Dataset " %}
If we choose to **Add a New Dataset**, this means we will create a new dataset with these three files we uploaded. We can always add new data to this dataset later.
{% endtab %}

{% tab title="Add to New Dataset" %}
If we choose **Add to Existing Dataset**, this means we will add these three NIFTI files to an existing dataset on BisQue. We are prompted with all of the datasets we have access to on BisQue: Datasets _we_ created and Datasets other _users_ have created but have made public.

<figure><img src="../../.gitbook/assets/existingdataset.png" alt=""><figcaption><p>Add uploaded data to an existing dataset.</p></figcaption></figure>
{% endtab %}
{% endtabs %}

### Step 4. Setting Permissions

Once we have uploaded the data, we can set permissions for who can access our data. By default, everything is uploaded as **Private**. If we want to make our data publically available, we can **Toggle Permissions**.

<figure><img src="../../.gitbook/assets/permissions.png" alt=""><figcaption></figcaption></figure>
