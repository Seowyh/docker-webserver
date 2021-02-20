# Build and Deploy a Webserver to Cloud Run

## Create the resources locally

Create the 4 files in this repository in your local folder:
1) package.json
2) index.js
3) Dockerfile
4) .github/workflows/GCP-Deploy.yml

## Google Cloud Console

1) Go to google cloud console  
[Google Cloud Console](https://console.cloud.google.com/)

2) Follow this tutorial
[Google Cloud Run with Github  Actions](https://cloud.google.com/community/tutorials/cicd-cloud-run-github-actions)  

3) under Cloud Run to obtain the followings:  
GCP_PROJECT_ID is your \$PROJECT_ID.  
GCP_APP_NAME is your app name.  
GCP_EMAIL is the email from the service account you created, which should look like this:   
\$ACCOUNT_NAME@$PROJECT_ID.iam.gserviceaccount.com  
GPC_CREDENTIALS is the content from the key.json file that you just created.


Step 1  

export PROJECT_ID=__*your own project unique ID*__
export ACCOUNT_NAME=__*your own account name*__
<br>
Step 6
gcloud iam service-accounts create $ACCOUNT_NAME \\
  --description="Cloud Run deploy account" \\
  --display-name="Cloud-Run-Deploy"

Step 7 execute one by one:

gcloud projects add-iam-policy-binding \$PROJECT_ID \\
  --member=serviceAccount:\$ACCOUNT_NAME@$PROJECT_ID.iam.gserviceaccount.com \\
  --role=roles/run.admin

gcloud projects add-iam-policy-binding \$PROJECT_ID \\
  --member=serviceAccount:\$ACCOUNT_NAME@$PROJECT_ID.iam.gserviceaccount.com \\
  --role=roles/storage.admin

gcloud projects add-iam-policy-binding \$PROJECT_ID \\
  --member=serviceAccount:\$ACCOUNT_NAME@$PROJECT_ID.iam.gserviceaccount.com \\
  --role=roles/iam.serviceAccountUser

Step 8 create key.json and copy out

gcloud iam service-accounts keys create key.json \\
    --iam-account \$ACCOUNT_NAME@$PROJECT_ID.iam.gserviceaccount.com

Enable container registry  
gcloud services enable cloudbuild.googleapis.com run.googleapis.com containerregistry.googleapis.com

create project in GCP  


## Github 

1) Create a github repository
2) Go to settings>secrets and enter to 4 secret values

## Commit and Push

1) Go to your local folder
2) Git add, commit and push to Github repo

You should see github actions building and deploying the webserver container to your Google Cloud run

## Verify 

1) Go to Google cloud platform. 
2) Under Cloud Run
3) Click on the app name
4) Click on the URL to verify webserver Hello World runs

## Cleanup

To avoid incurring charges, 
1) delete the app under Cloud Run thereafter
2) go to home>bucket resources and delete the buckets created for the app

