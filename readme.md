# Build and Deploy a Webserver to Cloud Run

## References

This repo is created based on the following reference tutorials. The first creates a docker container for a nodejs Webserver and deploy it to Cloud Run manually in GCP. Second tutorial uses Github Actions to build and deploy a Go app into GCP. In our case, we will build and deploy a nodejs Webserver by Github Actions into GCP Cloud Run.

[1) Deploy and run a container with Cloud Run on Node.js](https://codelabs.developers.google.com/codelabs/cloud-run-hello#0)

[2) Google Cloud Run with Github  Actions](https://cloud.google.com/community/tutorials/cicd-cloud-run-github-actions)  


## Google Cloud Console

### Step 1 Create a new Project  
Under Search products and resources  
> search Manage resources  
> Create project  

__*Or*__  Under Menu  _IAM>Manage Resources_

create project : _gitact_ __*(choose your own project ID)*__

### Step 2: Set variable and enable essential services  
Under Console terminal >  
>export PROJECT_ID=_gitact_ __*(project ID as above)*__
>export ACCOUNT_NAME=_gitacc_ __*(choose your own account name)*__  

>gcloud services enable cloudbuild.googleapis.com run.googleapis.com containerregistry.googleapis.com  

### Step 3: Create account  
>gcloud iam service-accounts create \$ACCOUNT_NAME \\
>  --description="Cloud Run deploy account" \\
>  --display-name="Cloud-Run-Deploy"

### Step 4: Set security settings  

>gcloud projects add-iam-policy-binding \$PROJECT_ID \\
>  --member=serviceAccount:\$ACCOUNT_NAME@$PROJECT_ID.iam.gserviceaccount.com \\
>  --role=roles/run.admin  

>gcloud projects add-iam-policy-binding \$PROJECT_ID \\
>  --member=serviceAccount:\$ACCOUNT_NAME@$PROJECT_ID.iam.gserviceaccount.com \\
>  --role=roles/storage.admin

>gcloud projects add-iam-policy-binding \$PROJECT_ID \\
>  --member=serviceAccount:\$ACCOUNT_NAME@$PROJECT_ID.iam.gserviceaccount.com \\
>  --role=roles/iam.serviceAccountUser

### Step 5: Create Credential (key.json)

>gcloud iam service-accounts keys create key.json \\
>    --iam-account \$ACCOUNT_NAME@$PROJECT_ID.iam.gserviceaccount.com

## Github 

### Step 1: Create a github repository

### Step 2: Enter the Secrets in repository
Go to settings>secrets and enter to 4 secret values  

GCP_APP_NAME=_your app name_ __*(your choice of app name)*__  
PROJECT_ID=_gitact_ __*(your project name) *__ 
GCP_EMAIL = _gitacc@gitact.iam.gserviceaccount.com_ __*(your client email from key.json)*__  
GCP_CREDENTIALS = __*key.json content*__  


## Create the resources locally

Copy or clone the 4 files in this repository to your local folder:
1) package.json
2) index.js
3) Dockerfile
4) .github/workflows/GCP-Deploy.yml



## Commit and Push

1) Go to your local folder
2) Git add, commit and push to Github repo

You should see github actions building and deploying the webserver container to your Google Cloud run

## Verify 

1) Go to Google cloud platform. 
2) Under Cloud Run
3) Click on the app name
4) Click on the URL to verify webserver Hello World is running

## Cleanup

To avoid incurring charges in GCP: 
1) delete the app under Cloud Run thereafter
2) go to home>bucket resources and delete the buckets created for the app

