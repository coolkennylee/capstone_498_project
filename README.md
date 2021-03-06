# capstone_498_project

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

![image](https://user-images.githubusercontent.com/16366387/120937709-ce7aaa80-c6c3-11eb-990d-c05cf64e40ea.png)
![image](https://user-images.githubusercontent.com/16366387/120937829-9162e800-c6c4-11eb-9285-d5ddf6521b43.png)
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

Import images from your bucket and click images once importing is complete. Make sure the images are labeled accordingly with the appropriate lables: 'no_mask' and 'yes_mask'

![image](https://user-images.githubusercontent.com/16366387/120936467-bf442e80-c6bc-11eb-8fc5-5ee8d6a57e5b.png)

Next, click train and do the following: 

(1) Give the model a name

(2) select Edge, so you can download the model offline which we will later put into our web app

(3) best trade-off

(4) set the number of nodes (3 - recommended, more if you'd like to train it faster which will cost more money)

![image](https://user-images.githubusercontent.com/16366387/120938132-3b8f3f80-c6c6-11eb-81cb-14098b78e740.png)
![image](https://user-images.githubusercontent.com/16366387/120938227-9d4fa980-c6c6-11eb-9da6-10d0ae0aa2d7.png)
![image](https://user-images.githubusercontent.com/16366387/120938268-bbb5a500-c6c6-11eb-9081-65c0444c9f64.png)


Once you click train, it's now just a waiting game until the training is complete. You can keep track of the status of your model in the models tab which will indicate whether the model is still training, errored or finished. You should be able to review the model results and you may retrain the model if you need to make tweaks to your model.

![image](https://user-images.githubusercontent.com/16366387/120936497-eb5faf80-c6bc-11eb-8266-58450ba26993.png)


After the training is compelte, you will need to export the deep learning model by clicking TensorFlow.js. You will need this file to include in your app portfolio.


![image](https://user-images.githubusercontent.com/16366387/120938295-d12acf00-c6c6-11eb-96a2-517965531218.png)

## App development

Creating an index.html and index.js file.

Utilize the index.html and index.js file in the github folders as a template for what the visuals should reflect. 

One important feature to note is making sure in the .js file that the following function reflects the approriate startPredicting() and stopPredicting() functions. This particular function will output the key results that will determine the probably of whether the person has a mask or not in real time.
``` 
document.getElementById("predictions-mask").innerText=result['0']['label']+": "+Math.round(result['0']['prob']*100)+"%";
 document.getElementById("predictions-no-mask").innerText=result['1']['label']+": "+Math.round(result['1']['prob']*100)+"%";
```
![image](https://user-images.githubusercontent.com/16366387/120939973-96796480-c6cf-11eb-89e9-5cd14f24a7d3.png)

## Hosting the application on GCP

Aggregate your files into one folder. You can name it www or whatever suits the best for you. Next, create an app.yaml file and make sure the following code reflects the following

``` runtime: python27
api_version: 1
threadsafe: true

handlers:
- url: /
  static_files: www/index.html
  upload: www/index.html

- url: /(.*)
  static_files: www/\1
  upload: www/(.*)
  ```
  
Save the file and upload your file to cloud shell editor. The left side panel should reflect the following files:

![image](https://user-images.githubusercontent.com/16366387/120940098-4a7aef80-c6d0-11eb-85d4-b78d32f46134.png)

After, go back to cloud shell and execute the following code to enable Google App Engine and to deploy the app to a webpoint.

![image](https://user-images.githubusercontent.com/16366387/120940154-b8271b80-c6d0-11eb-9ae3-4f00d0d2c095.png)

![image](https://user-images.githubusercontent.com/16366387/120940180-df7de880-c6d0-11eb-8195-c251784f8639.png)

Copy and paste the web link in a browser and it should take you to the application. Click the green button and wait a few moments for the camera to turn on (may need to approve and enable webcam on your browser).

![image](https://user-images.githubusercontent.com/16366387/120940297-68951f80-c6d1-11eb-90e2-c5140562d04b.png)



## CI/CD

Enable the Stackdriver API from Google's API and service search bar. You should be able to monitor and log your app activities. It's highly recommended to implement alerts/triggers on stackdriver to notify adminstrators on any downtime time of the application or sudden increase in traffic.

![image](https://user-images.githubusercontent.com/16366387/120940497-6d0e0800-c6d2-11eb-9141-a34e7dd98bd6.png)

![image](https://user-images.githubusercontent.com/16366387/120940512-7e571480-c6d2-11eb-9181-617cf1e6f08d.png)


Set up Cloud Build to auto deploy to your application when you make any changes on Github. Make sure to connect repository and select source and go through the authentication process to import and mirror the target GitHub repo you plan to ingest into Cloud Build.

![image](https://user-images.githubusercontent.com/16366387/120940726-882d4780-c6d3-11eb-9c37-029add9c4f86.png)

## Conclusion

Thanks for checking out my project and I hope you're able to build your vision app that detects if you're wearing a mask or not! 



