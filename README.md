# capstone_498_group_project

In this tutorial, we will help fellow students and colleagues learn how to build and deploy a computer vision model that detects if someone is wearing a mask or not. We will utilize Google Cloud Platform as our core serverless infrastructure to build, train/model, and deploy an interactive application.

## Steps we will cover in this tutorial

(1) Setting up Google Cloud Platform and setting up a project

(2) Enabling required APIs such as: AutoML Vision API, App Engine, Cloud Build, and Stackdriver

(3) How to source/create datasets 

(4) Training, evaluating and exporting the model

(5) Building the application

(6) Deploying the application

(7) Implementing DevOps to implement automated, seamless CI/CD process


## Summary of the project & strategy

![image](https://user-images.githubusercontent.com/16366387/120933218-f8c16d80-c6ad-11eb-8748-f89a2c4ab984.png)
![image](https://user-images.githubusercontent.com/16366387/120933089-64570b00-c6ad-11eb-809d-687cc4bbced1.png)
![image](https://user-images.githubusercontent.com/16366387/120933094-6a4cec00-c6ad-11eb-939b-15b17e63a83c.png)
![image](https://user-images.githubusercontent.com/16366387/120933104-720c9080-c6ad-11eb-8a63-e992b2072dbc.png)

## Configuring GCP

First, you need to make sure you have a seperate project available for this application. Click on create new project.

![image](https://user-images.githubusercontent.com/16366387/120933681-07108900-c6b0-11eb-9b20-c5926ab0e098.png)


## Enabling Required APIs

Enable APIs by clicking on API & Services on the console and click enable, which will take you to a search engine. Type each API accordingly and click enable API.

(1) AutoML Vision

(2) App Engine + App Engine Admin API

(3) Cloud Build

(4) Stackdriver API

![image](https://user-images.githubusercontent.com/16366387/120934165-0ed12d00-c6b2-11eb-8c33-1651c3e7d650.png)

![image](https://user-images.githubusercontent.com/16366387/120934886-09c1ad00-c6b5-11eb-998a-4a1dee0d3a78.png)

## Dataset

There are a few ways you can source image data for free or low-cost

(1) Existing, open-source github repos
    
* https://github.com/chandrikadeb7/Face-Mask-Detection/tree/master/dataset
* https://github.com/topics/mask-detection-dataset


(2) Kaggle practice dataset

* https://www.kaggle.com/omkargurav/face-mask-dataset
* https://www.kaggle.com/andrewmvd/face-mask-detection

(3) Bing image search API via

For this section you will need to do two things: (1) Create an Azure account and enable Bing's Image Search API and obtain the API key.

* You can create your account and enable/obtain your API key in this link: https://www.microsoft.com/en-us/bing/apis/bing-image-search-api
* Build a search python script, which is included in this github repo in the search_image.py file. You will need to make adjustments to the file and add your own API key, search parameters and the URL reflects the latest version of bing's image search API.

![image](https://user-images.githubusercontent.com/16366387/120934797-9b7cea80-c6b4-11eb-8c14-3a2858bd9cba.png)


You can either create a Google Cloud Storage bucket or download the data as a zip file and upload it when we start building the model which will give you the option to create the cloud storage bucket then.

You can type the following in the CloudShell command line to create a new cloud storage bucket or wait until the next step.

``` 
gsutil mb -p $PROJECT_ID \ 
   -c standard    \
   -l us-central1 \
   gs://$PROJECT_ID-vcm/ 
```

## Training, modeling, evaluating, and exporting TensorFlow model

In your GCP console, find the AutoML Vision and click on dataset. Next, click on multi-label classification and give your dataset a name to reference during trianing.

![image](https://user-images.githubusercontent.com/16366387/120935781-3f689500-c6b9-11eb-915b-ac1fc18a1fe0.png)

You can either upload the zipped/unzipped dataset or if you already ingested the data into Google Cloud Storage, then you can select the file destination through the browse function.

![image](https://user-images.githubusercontent.com/16366387/120935788-4db6b100-c6b9-11eb-840b-a170d0d0eea8.png)

When designing your cloud storage, make sure to enable multi-region -> default storage class as 'standard', uniform access control, google-managed encryption key, and click create bucket (should take a several minutes depending on how many images you're uploading or sourcing).

![image](https://user-images.githubusercontent.com/16366387/120935897-c61d7200-c6b9-11eb-942a-0c97e35c48e0.png)


## App development

Creating an index.html and index.js file

## Hosting the application on GCP

Using CloudShell to import app files and deploying GCP App Engine

## CI/CD

Stackdriver

Cloud Build





