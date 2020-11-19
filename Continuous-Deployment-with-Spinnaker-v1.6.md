## Kubernetes GCP commands
[source](https://googlecoursera.qwiklabs.com/focuses/11593456?parent=lti_session)

### GCP instance: k8s-workshop-module-1-lab

## Objectives
Set up your environment by launching Cloud Shell and deploying Spinnaker for Google Cloud.

Create a second GKE cluster to deploy the sample application to.

Configure Spinnaker pipelines to reliably and continuously deploy your app to GKE environments.

Copy a sample application, create a Git repository, and upload it to a Cloud Source Repositories.

Create a Cloud Build trigger that builds Docker images when your application changes.

Deploy a code change, watch your trigger build a Docker image, then observe your Spinnaker pipleline roll it out to staging and production.

Roll back your change.

Pipeline architecture
pipeline architecture

To continuously deliver app updates to your users, you need an automated process that reliably builds, tests, and updates your software. Code changes should automatically flow through a pipeline that includes artifact creation, unit testing, functional testing, and production rollout. In some cases, you want a code update to apply to only a subset of your users, so that it is exercised realistically before you push it to your entire user base. If one of these canary releases proves unsatisfactory, your automated procedure must be able to quickly roll back the software changes.

With GKE and Spinnaker, you can create a robust continuous delivery flow that helps to ensure your software is shipped as quickly as it is developed and validated. Although rapid iteration is your end goal, you must first ensure that each app revision passes through a series of automated validations before becoming a candidate for production rollout. When a given change has been vetted through automation, you can also validate the app manually and conduct further prerelease testing.

After your team decides the app is ready for production, one of your team members can approve it for production deployment.

App delivery pipeline
In this lab, you build the continuous delivery pipeline shown in the following diagram.

App delivery pipeline

The high-level steps of this pipeline are as follows:

1. A developer changes code and pushes it to a repository.
2. Cloud Build detects the changes, builds the Docker image, tests the image, and pushes the image to Spinnaker.
3. Spinnaker detects the image, deploys the image to staging, and tests the staging deployment. After a manual approval, Spinnaker deploys the image to production.


## Setup

### Step 1
For each lab, you get a new GCP project and set of resources.

### Step 2
Make sure the following APIs are enabled in Cloud Platform Console:
- Kubernetes Engine API
- Container Registry API
On the **Navigation menu** (Navigation menu), click **APIs & services**.

### Step 3
## Activate Google Cloud Shell

Google Cloud Shell is a virtual machine that is loaded with development tools. It offers a persistent 5GB home directory and runs on the Google Cloud. Google Cloud Shell provides command-line access to your GCP resources.

1. In GCP console, on the top right toolbar, click the Open Cloud Shell button.
2. Click Continue.

It takes a few moments to provision and connect to the environment. When you are connected, you are already authenticated, and the project is set to your PROJECT_ID.

**gcloud** is the command-line tool for Google Cloud Platform. It comes pre-installed on Cloud Shell and supports tab-completion. You can list the active account name with this command:<br>
`gcloud auth list`

You can list the project ID with this command:
`gcloud config list project`

### Step 4
Creating a cluster for Spinnaker operation
In this section, you configure the infrastructure required to complete the lab.

Deploy the Spinnaker for Google Cloud solution using Cloud Shell
With Spinnaker for Google Cloud, you can set up and manage Spinnaker in a production-ready configuration, optimized for Google Cloud. Spinnaker for Google Cloud sets up resources (GKE, Memorystore, Cloud Storage buckets and service accounts), integrates Spinnaker with related services such as Cloud Build, and provides a Cloud Shell-based management environment for your Spinnaker installations, with helpers and common tools such as spin and hal.

Clone the Spinnaker for Google Cloud repository into your Cloud Shell environment.

mkdir ~/cloudshell_open && cd ~/cloudshell_open

git clone https://github.com/GoogleCloudPlatform/spinnaker-for-gcp.git

cd spinnaker-for-gcp

Create a script with your Spinnaker environment properties.

./scripts/install/setup_properties.sh

This creates the ./scripts/install/properties file.

Review your Spinnaker environment properties.

cat ./scripts/install/properties

Apply the properties to your environment.

source ./scripts/install/properties

Set up a git user which is used to create a git repo for Spinnaker config.

git config --global user.email "$USER@qwiklabs.net"
git config --global user.name "$USER"

Start the Spinnaker installation script.

./scripts/install/setup.sh

Necessary APIs are enabled, which takes a couple minutes, and then you can follow along with the script output to see the steps:

A service account is created and IAM roles assigned for Spinnaker tasks
A Redis instance is created in Cloud Memorystore
A Cloud Storage bucket is created
A GKE cluster is created for Spinnaker application pipelines
PubSub topics and subscriptions are created
Spinnaker resources are provisioned in the GKE cluster
A Spinnaker config git repo is configured in Cloud Source Repositories
The Halyard management tool for configuring Spinnaker: hal is installed in Cloud Shell
The Spinnaker tool for managing applications and pipelines as code: spin is installed in Cloud Shell
Altogether these tasks can take >15min to complete. You may see warnings that you can safely ignore as they should have no impact on this lab.

Note: This installation command will take several minutes to complete and is a basic setup for the purposes of this tutorial. For a production setup, follow the detailed instructions included with Spinnaker for Google Cloud.
Restart Cloud Shell to load new environment settings.

Click Restart from the Cloud Shell more menu, then Restart again. You don't have to select a reason for restarting.

restart cloudshell

Restore your environment and directory.

cd ~/cloudshell_open/spinnaker-for-gcp

source ./scripts/install/properties

Connect to Spinnaker UI
Set up port-forwarding to connect from Cloud Shell.

./scripts/manage/connect_unsecured.sh

Note: If you encounter error: You must be logged in to the server Unauthorized)() re-run the connect_unsecured.sh command.
Open the Spinnaker web user interface.

In Cloud Shell, click the Web Preview icon and select Preview on port 8080.

port8080

You should see the welcome screen, followed by the Spinnaker UI:

hello

spinui

Note: For now, this Spinnaker instance isn't publicly accessible. Only you have access to it, with no authentication. A production Spinnaker instance is a critical component of your infrastructure, so you must properly secure it. Several options are available to you for security and authentication:
Spinnaker for Google Cloud provides tools to help secure your deployment using IAP with a SSL Certificate.
Take a look at the security documentation of Spinnaker.
Use G Suite as an identity provider for Spinnaker authentication.
Use Google Groups for Spinnaker authorization.
Configure a Identity-Aware Proxy in front of Spinnaker to further control who has access to it.
Click Check my progress to verify the objective.
Set up the environment

Creating a second GKE cluster for application deployments
A common pattern is to have a GKE cluster used for builds, deployments, and so on, and then other GKE clusters for running applications. In this section you create another GKE cluster, app-cluster, to deploy the sample application to.

In Cloud Shell, create a new GKE cluster for your application.

For this example, use a different region than the one used by the Spinnaker deployment.

APP_REGION=us-west1-b; gcloud config set compute/zone $APP_REGION

gcloud container clusters create app-cluster --machine-type=n1-standard-2

This can take ~4 min to complete. You may see warnings that you can safely ignore as they should have no impact on this lab.

Grant Spinnaker access to the new applications cluster.

Confirm the default script values by pressing [Enter] when prompted by each of three questions.

./scripts/manage/add_gke_account.sh

Example output and values:

Please enter the context you wish to use to manage your GKE resources: gke_spinnaker-246920_us-west1-b_app-cluster

Please enter the id of the project within which the referenced cluster lives: spinnaker-246920

Please enter a name for the new Spinnaker account: app-cluster-acct

This ensures that a Google Cloud service account, named in your Spinnaker properties file, has the proper roles to access the application cluster. Then that service account is used to create Kubernetes credentials named app-cluster-acct in the Spinnaker configuration.

Apply this updated configuration to your Spinnaker cluster.

kubectl config use-context gke_${PROJECT_ID}_${ZONE}_spinnaker-1

./scripts/manage/push_and_apply.sh

Click Check my progress to verify the objective.
Creating a second GKE cluster for application deployments

Configuring your deployment pipelines
Deploy two Spinnaker pipelines that respond when new application images are available. The first pipeline is for a staging environment for integration testing of changes.

After the integration tests pass, you must manually approve the changes to allow the updated image to deploy to the production environment.

configure deployment pipeline

Create the application and deployment pipelines in Spinnaker
Use spin to register the helloworldwebapp in Spinnaker.

cd ./samples/helloworldwebapp/

~/spin app save --application-name helloworldwebapp \
    --cloud-providers kubernetes --owner-email $IAP_USER

Use spin to create a "Deploy to Staging" Spinnaker pipeline.

cat templates/pipelines/deploystaging_json.template | envsubst  > templates/pipelines/deploystaging.json

~/spin pi save -f templates/pipelines/deploystaging.json

Save the created staging pipeline id.

The staging pipeline id is used by the production pipeline as a trigger reference. By saving this id the production pipeline will automatically start work when it notices the staging pipeline has completed successfully.

export DEPLOY_STAGING_PIPELINE_ID=$(~/spin pi get -a helloworldwebapp -n 'Deploy to Staging' | jq -r '.id')

Use spin to create a "Deploy to Prod" Spinnaker pipeline.

cat templates/pipelines/deployprod_json.template | envsubst  > templates/pipelines/deployprod.json

~/spin pi save -f templates/pipelines/deployprod.json

You've created your application and pipelines in Spinnaker. You'll explore your pipelines in a later step.

Click Check my progress to verify the objective.
Configuring your deployment pipelines

Creating your sample application code
In this section, you copy a sample application, create a Git repository for it in Cloud Source Repositories, then push the application source to the repo.

You then configure Cloud Build to detect changes to your application source code, build a Docker image, and then push it to Container Registry.

Create your application source and config files
In Cloud Shell, create a new directory for your application source.

mkdir -p ~/$PROJECT_ID/spinnaker-for-gcp-helloworldwebapp/

Copy the application source files.

cp -r templates/repo/src ~/$PROJECT_ID/spinnaker-for-gcp-helloworldwebapp/

Copy the application config files.

cp -r templates/repo/config ~/$PROJECT_ID/spinnaker-for-gcp-helloworldwebapp/

cp templates/repo/Dockerfile ~/$PROJECT_ID/spinnaker-for-gcp-helloworldwebapp/

cat templates/repo/cloudbuild_yaml.template | envsubst '$BUCKET_NAME' > ~/$PROJECT_ID/spinnaker-for-gcp-helloworldwebapp/cloudbuild.yaml
cat ~/$PROJECT_ID/spinnaker-for-gcp-helloworldwebapp/config/staging/replicaset_yaml.template | envsubst > ~/$PROJECT_ID/spinnaker-for-gcp-helloworldwebapp/config/staging/replicaset.yaml
rm ~/$PROJECT_ID/spinnaker-for-gcp-helloworldwebapp/config/staging/replicaset_yaml.template
cat ~/$PROJECT_ID/spinnaker-for-gcp-helloworldwebapp/config/prod/replicaset_yaml.template | envsubst > ~/$PROJECT_ID/spinnaker-for-gcp-helloworldwebapp/config/prod/replicaset.yaml
rm ~/$PROJECT_ID/spinnaker-for-gcp-helloworldwebapp/config/prod/replicaset_yaml.template

Note the config files include your Kubernetes application manifests. Spinnaker needs access to your Kubernetes manifests in order to deploy the application to your clusters during pipeline execution.

Create your application source/config Git repository
Change directories to the source/config code directory.

cd ~/$PROJECT_ID/spinnaker-for-gcp-helloworldwebapp

Make the initial commit to your source code repository.

git init
git add .
git commit -m "Initial commit"

Create a repository to host your code:

gcloud source repos create spinnaker-for-gcp-helloworldwebapp
git config credential.helper gcloud.sh

Add your newly created repository as remote:

git remote add origin https://source.developers.google.com/p/$PROJECT_ID/r/spinnaker-for-gcp-helloworldwebapp

Push your code to the new repository's master branch:

git push origin master

If you want, review your source code using Cloud Console by clicking Navigation menu, then in the Tools section, click Source Repositories.

Click View All repositories then spinnaker-for-gcp-helloworldapp.

You can look through the files.

csr files

Click Check my progress to verify the objective.
Create your application source/config Git repository

Configuring your build triggers
In this section, you configure Cloud Build to build and push your Docker image every time you push an update to the master branch of your source. Cloud Build automatically checks out your source code, builds the Docker image from the Dockerfile in your repository, and pushes that image to Container Registry.

Trigger workflow

Review your Cloud Build steps.

cat ./cloudbuild.yaml

Cloud Build will build your application, generate Kubernetes config manifests for multiple environments, and produce an image file.

Create the Cloud Build trigger.

gcloud beta builds triggers create cloud-source-repositories \
    --repo spinnaker-for-gcp-helloworldwebapp \
    --branch-pattern master \
    --build-config cloudbuild.yaml \
    --included-files "src/**,config/**"

Verify the Cloud Build trigger.

gcloud beta builds triggers list

Output (do not copy)

---
createTime: '2020-03-03T04:51:57.768831415Z'
filename: cloudbuild.yaml
id: 7e8cbeb3-8678-4bbc-b848-d3b6035f6c3f
includedFiles:
- src/**
- config/**
name: trigger
triggerTemplate:
  branchName: master
  projectId: qwiklabs-gcp-01-3367ef948325
  repoName: spinnaker-for-gcp-helloworldwebapp

From now on, whenever you push to the master branch of your source code repository, Cloud Build automatically builds and pushes your app as a Docker image to Container Registry.

Click Check my progress to verify the objective.
Configuring your build triggers

Building your image
Push your first image to Container Registry using the following steps.

In Cloud Shell, from your source code directory, create a small change.

sed -i 's/Hello World/Hello World 1.0/g' ./src/main.go

git diff

git commit -a -m "Change to 1.0"

Push the change which will trigger a build.

git push origin master

Check Cloud Build in Cloud Console for the triggered build.

Click Navigation menu, then in the Tools section, click Cloud Build > Dashboard.

Look for a build with status Running.

Stay on this page and wait for the build to complete before going on to the next step.

After ~1 minute you should see Successful.

If you don't see Successful, wait a bit and refresh. Verify the trigger was configured properly in the previous instructions.

Build success

Click Check my progress to verify the objective.
Building the Docker image

Viewing your pipelines
The Spinnaker configuration you created uses notifications of new images being pushed to trigger Spinnaker pipelines.

You pushed a change to Cloud Source Repositories which triggered Cloud Build to build and push your image to Container Registry.

Review the Deploy to Staging pipeline
Open your application in the Spinnaker UI.

Return to the Spinnaker UI and click Applications at the top of the screen to see your list of managed applications.

helloworldwebapp is your application. If you don't see helloworldwebapp, try refreshing the Spinnaker Applications tab.

spinnaker_applications

Click helloworldwebapp to view your application deployment.

Click Pipelines at the top to view your application pipeline status.

You should notice two pipelines: Deploy to Staging and Deploy to Production.

Deploy to Staging should complete with ~2 minutes.

spinnaker_pipelines

Review staging execution details.

When the pipeline is completed, all of the stage boxes should be colored green to signify success running each.

Click Execution Details on the Deploy to Staging pipeline.

Look for Status SUCCEEDED.

spinnaker staging details

Review the Deploy to Production pipeline
In Pipelines, close Execution Details on Deploy to Staging and click Execution Details on the Deploy to Production pipeline.

spinnaker production details

Notice the production pipeline is paused. It's set to await a Manual Judgement before proceeding.

Allow the production deployment to proceed.

Below the question Continue with deployment?, click Continue.

This unblocks the pipeline. You can follow the pipeline executing while it has status of RUNNING.

Click a stage to see details about it.

Click Deploy Namespace to see up to date status of the stage. Click Task Status and Artifact Status too.

Scroll the pipeline right to see all possible stages.

Notice Validation Succeeded becomes green when your pipeline is done.

View the running production application
Click Infrastructure > Load Balancers at the top of the Spinnaker UI.

load balancers

Under service-helloworldwebapp-service, click HELLOWORLDWEBAPP-PROD.

On the right under STATUS, copy the Ingress ip-address by clicking Copy Ingress IP to clipboard.

production ingress

The Ingress IP link from the Spinnaker UI uses HTTP.

Paste the address into your browser to view the production version of the application. You may need to make sure it is an HTTP address.

production ingress green

You have now verified building, testing, and deploying your application.

Click Check my progress to verify the objective.
Viewing your pipelines

Triggering your pipeline from code changes
In this section, you test the pipeline end to end by making a code change, and watching the pipeline run in response.

Again, when you push a change, you trigger Cloud Build to build a new Docker image and push it to Container Registry. Spinnaker detects that a new image was created and triggers a pipeline to deploy the image to staging, run tests, and roll out the same image to all pods in the deployment.

Change the color of the application background to blue.

In Cloud Shell, from spinnaker-for-gcp-helloworldwebapp, make a small code change.

sed -i 's/green/blue/g' ./src/main.go

git diff

git commit -a -m "Change to blue."

Push the change which will trigger a build.

git push origin master

Check Cloud Build in Cloud Console for the triggered build.

Click Navigation menu, then in the Tools section, click Cloud Build > Dashboard.

Look for a build with status Running.

Stay on this page and wait for the build to complete before going on to the next step.

After ~1 minute you should see Successful.

Return to the Spinnaker UI and click Pipelines to watch the staging pipeline start to deploy the image.

Approve the paused production pipeline.

Hover over the yellow stage indicator then click Continue.

production paused

Wait for all stages to complete.

Continue to the next step once the Validation Succeeded stage is completed.

Verify your change was deployed.

Refresh your production application page and notice the background is now blue.

production ingress blue

Click Check my progress to verify the objective.
Triggering your pipeline from code changes

Rolling back a change by reverting your previous commit
Roll back the latest change.

git revert --no-edit HEAD

Reverting creates a new commit undoing the latest commit.

Push the change which will trigger a build.

git push origin master

As before, wait a few minutes for the build to complete.

As before, use the Spinnaker UI to watch the staging pipeline complete.

As before, confirm the prompt to deploy to production.

Wait for the reverted changes to be fully deployed.

Wait for the stage Scale Down Old Prod to be completed.

Verify the roll back by refreshing the application UI.

Refresh your application UI, open to the Ingress IP.

Look for the green background again.

production ingress green

Congratulations
You have now successfully completed the Continuous Deployment with Spinnaker lab.




