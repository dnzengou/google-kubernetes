# Lab1: Accessing the Google Cloud Console and Cloud Shell
## [source](https://googlecoursera.qwiklabs.com/focuses/12423222?parent=lti_session)

Username: student-01-ef8f88616d86@qwiklabs.net
Password: WVptS8yTH5X
GCP Project ID: qwiklabs-gcp-01-79c0b5cf7db1

***

1 hour
Free
Rate Lab
Overview
In this lab, you become familiar with the Google Cloud web-based interface. Two integrated environments are available:

A GUI environment called the Google Cloud Console
A command-line interface called Cloud Shell, which has the commands from the Cloud SDK pre-installed
In this course, you use both environments.

You need to know a few things about the Google Cloud Console:

The Google Cloud Console is under continuous development, so the graphical layout occasionally changes. Often, these changes are made to accommodate new Google Cloud features or changes in the technology, resulting in a slightly different workflow.

You can perform most common Google Cloud actions in the Google Cloud Console. Sometimes new features are implemented in the Cloud SDK before they are made available in the Google Cloud Console.

The Google Cloud Console is extremely fast for some activities. The Google Cloud Console can perform multiple actions on your behalf that might require many command-line actions.

The commands in the Cloud SDK are valuable tools for automation.

Objectives
In this lab, you learn how to perform the following tasks:

Learn how to access the Google Cloud Console and Cloud Shell

Become familiar with the Google Cloud Console

Become familiar with Cloud Shell features, including the Cloud Shell code editor

Use the Google Cloud Console and Cloud Shell to create buckets and VMs and service accounts

Perform other commands in Cloud Shell

*** 

Task 0. Lab Setup
Access Qwiklabs
For each lab, you get a new GCP project and set of resources for a fixed time at no cost.

Make sure you signed into Qwiklabs using an incognito window.

Note the lab's access time (for example, img/time.png and make sure you can finish in that time block.

There is no pause feature. You can restart if needed, but you have to start at the beginning.

When ready, click img/start_lab.png.

Note your lab credentials. You will use them to sign in to Cloud Platform Console. img/open_google_console.png

Click Open Google Console.

Click Use another account and copy/paste credentials for this lab into the prompts.

If you use other credentials, you'll get errors or incur charges.

Accept the terms and skip the recovery resource page.
Do not click End Lab unless you are finished with the lab or want to restart it. This clears your work and removes the project.

After you complete the initial sign-in steps, the project dashboard appears.

Google Cloud Project Dashboard

Task 1. Explore the Google Cloud Console
In this task, you explore the Google Cloud Console and create resources.

Verify that your project is selected
In the Select a project drop-down list in the title bar select the project ID that Qwiklabs provided with your authentication credentials.
The project ID will resemble qwiklabs-Google Cloud- followed by a long hexadecimal number.

Click Cancel to close the dialog.
Your title bar should indicate the project ID as shown in the screenshot. Each lab in the Qwiklabs environment has a unique project ID, as well as unique authentication credentials.

Google Cloud project ID
Navigate to Google Cloud Storage and create a bucket
Cloud Storage allows world-wide storage and retrieval of any amount of data at any time. You can use Cloud Storage for a range of scenarios including serving website content, storing data for archival and disaster recovery, or distributing large data objects to users via direct download.

Cloud Storage buckets must have a globally unique name. In your organization, you should follow Google Cloud's recommended best practices for naming buckets. For this lab, we can easily get a unique name for our bucket by using the ID of the Google Cloud project that Qwiklabs created for us, because Google Cloud project IDs are also globally unique.

In the Google Cloud Console, on the Navigation menu (Navigation menu), click Home .
In the Dashboard tab of the resulting screen, the Project info section shows your Google Cloud project ID. Select and copy the project ID. Because this project ID was created for you by Qwiklabs, it will resemble qwiklabs-Google Cloud- followed by a long hexadecimal number.
In the Google Cloud Console, on the Navigation menu (Navigationmenu), click Storage > Browser.
Click Create bucket.
For Name, paste in the Google Cloud project ID string you copied in an earlier step. Leave all other values as their defaults.
These lab instructions will later refer to the name that you typed as [BUCKET_NAME].

Click Create.
The Google Cloud Console has a Notifications (notifications icon) icon. Feedback from the underlying commands is sometimes provided there. You can click the icon to check the notifications for additional information and history.

Create a virtual machine (VM) instance
Google Compute Engine offers virtual machines running in Google's datacenters and on its network as a service. Google Kubernetes Engine makes use of Compute Engine as a component of its architecture. For this reason, it's helpful to learn a bit about Compute Engine before learning about Kubernetes Engine.

On the Navigation menu (Navigation menu ), click Compute Engine > VM instances.
Click Create Instance.
For Name, type first-vm as the name for your instance.
For Region, select us-central1.
For Zone, select us-central1-c.
For Machine type, examine the options.
The Machine type: menu lists the number of virtual CPUs, the amount of memory, and a symbolic name such as n1-standard-1. The symbolic name is the parameter you use to select the machine type when using the gcloud command to create a VM. To the right of the region, zone, and machine type is a per-month estimated cost.

To see the breakdown of estimated costs, click Details to the right of the Machine type list underneath the estimated costs.
For Machine type, click 2 vCPUs (n1-standard-2).
How did the cost change?

For Machine type, click f1-micro (1 shared vCPU).
The micro type is a shared-core VM that is inexpensive.

For Firewall, click Allow HTTP traffic.
Leave the remaining settings as their defaults, and click Create.
Wait until the new VM is created.

Explore the VM details
On the VM instances page, click the name of your VM, first-vm.
Locate CPU platform, notice the value, and click Edit.
You can't change the machine type, the CPU platform, or the zone of a running Google Cloud VM. You can add network tags and allow specific network traffic from the internet through firewalls.

Some properties of a VM are integral to the VM and are established when the VM is created. They cannot be changed. Other properties can be edited. For example, you can add disks, and you can determine whether the boot disk is deleted when the instance is deleted.

Scroll down and examine Availability policies.
Compute Engine offers preemptible VM instances, which cost less per hour but can be terminated by Google Cloud at any time. These preemptible instances can save you a lot of money, but you must make sure that your workloads are suitable to be interrupted. You can't convert a non-preemptible instance into a preemptible one. This choice must be made at VM creation.

If a VM is stopped for any reason (for example, an outage or a hardware failure), the automatic restart feature starts it back up. Is this the behavior you want? Are your applications idempotent (written to handle a second startup properly)?

During host maintenance, the VM is set for live migration. However, you can have the VM terminated instead of migrated.

If you make changes, they can sometimes take several minutes to be implemented, especially if they involve networking changes, like adding firewalls or changing the external IP.

Click Cancel.

Create an IAM service account
An IAM service account is a special type of Google account that belongs to an application or a virtual machine, instead of to an individual end user.

On the Navigation menu, click IAM & admin > Service accounts.
Click + Create service account.
On the Service account details page, specify the Service account name as test-service-account.
Click Create.
On the Service account permissions page, specify the role as Project > Editor.
Click Continue.
Click Done.
On the Service accounts page, move to the extreme right of the test-service-account and click on the three dots.
Click Create Key.
Select JSON as the key type.
Click Create.
A JSON key file is downloaded. In a later step, you find this key file and upload it to the VM.

Click Close.
Click Check my progress to verify the objective.
Create a bucket, VM instance with necessary firewall rule and an IAM service account.

Task 2. Explore Cloud Shell
Cloud Shell provides you with command-line access to your cloud resources directly from your browser. With Cloud Shell, Cloud SDK command-line tools such as gcloud are always available, up to date, and fully authenticated.

Cloud Shell provides the following features and capabilities:

Temporary Compute Engine VM
Command-line access to the instance through a browser
5 GB of persistent disk storage ($HOME dir)
Preinstalled Cloud SDK and other tools
gcloud: for working with Compute Engine, Google Kubernetes Engine (GKE) and many Google Cloud services
gsutil: for working with Cloud Storage
kubectl: for working with GKE and Kubernetes
bq: for working with BigQuery
Language support for Java, Go, Python, Node.js, PHP, and Ruby
Web preview functionality
Built-in authorization for access to resources and instances
After 1 hour of inactivity, the Cloud Shell instance is recycled. Only the /home directory persists. Any changes made to the system configuration, including environment variables, are lost between sessions.

In this task, you use Cloud Shell to create and examine some resources.

Open Cloud Shell and explore its features
On the Google Cloud Console title bar, click Activate Cloud Shell (e92fcd01cbb5e0ff.png).
When prompted, click Continue.
Cloud Shell opens at the bottom of the Google Cloud Console window.

The following icons are on the far right of Cloud Shell toolbar:

Hide/Restore: This icon hides and restores the window, giving you full access to the Google Cloud Console without closing Cloud Shell.

Open in new window: Having Cloud Shell at the bottom of the Google Cloud Console is useful when you are issuing individual commands. But when you edit files or want to see the full output of a command, clicking this icon displays Cloud Shell in a full-sized terminal window.

Close all tabs: This icon closes Cloud Shell. Everytime you close Cloud Shell, the virtual machine is recycled and all machine context is lost. However, data that you stored in your home directory is still available to you the next time you start Cloud Shell.

Use Cloud Shell to set up the environment variables for this task
In Cloud Shell, use the following commands to define the environment variables used in this task.

Replace [BUCKET_NAME] with the name of the first bucket from task 1.
Replace [BUCKET_NAME_2] with a globally unique name.
You can append a 2 to the globally unique bucket name that you used previously.

MY_BUCKET_NAME_1=[BUCKET_NAME]
MY_BUCKET_NAME_2=[BUCKET_NAME_2]
MY_REGION=us-central1
When you are working in the Cloud Shell or writing scripts, creating environment variables is a good practice. You can easily and consistently re-use these environment variables, which makes your work less error-prone.

Make sure you replace the full placeholder string, such as [BUCKET_NAME]with the unique name that you choose, for example MY_BUCKET_NAME_1=unique_bucket_name.

Move the credentials file you created earlier into Cloud Shell
You downloaded a JSON-encoded credentials file in an earlier task when you created your first Cloud IAM service account.

On your local workstation, locate the JSON key that you just downloaded and rename the file to credentials.json.

In Cloud Shell, click the three dots ( Three-dot menu icon) icon in the Cloud Shell toolbar to display further options.

Click Upload file and upload the credentials.json file from your local machine to the Cloud Shell VM.

Click the X icon to close the file upload pop-up window.

In Cloud Shell, type ls and press ENTER to confirm that the file was uploaded.

Create a second Cloud Storage bucket and verify it in the Google Cloud Console
The gsutil command, which is supplied by the Cloud SDK, lets you work with Cloud Storage from the command line. In this task, you use the gsutil command in Cloud Shell.

In Cloud Shell, use the gsutil command to create a bucket.

gsutil mb gs://$MY_BUCKET_NAME_2
In the Google Cloud Console, on the Navigation menu (Navigation menu), click Storage > Browser, or click Refresh if you are already in the Storage Browser.
The second bucket should appear in the Buckets list.

Use the gcloud command line to create a second virtual machine
In Cloud Shell, execute the following command to list all the zones in a given region:

gcloud compute zones list | grep $MY_REGION
Select a zone from the first column of the list. Notice that Google Cloud zones' names consist of their region name, followed by a hyphen and a letter.
You may choose a zone that is the same as or different from the zone that you used for the first VM in task 1.

Execute the following command to store your chosen zone in an environment variable.
You replace [ZONE] with your selected zone.

MY_ZONE=[ZONE]
Set this zone to be your default zone by executing the following command.

gcloud config set compute/zone $MY_ZONE
Execute the following command to store a name in an environment variable you will use to create a VM. You will call your second VM second-vm.

MY_VMNAME=second-vm
Create a VM in the default zone that you set earlier in this task using the new environment variable to assign the VM name.

gcloud compute instances create $MY_VMNAME \
--machine-type "n1-standard-1" \
--image-project "debian-cloud" \
--image-family "debian-9" \
--subnet "default"
List the virtual machine instances in your project.

gcloud compute instances list
You will see both your newly created and your first virtual machine in the list.

In the Google Cloud Console, on the Navigation menu ( Navigation menu), click Compute Engine > VM Instances. Just as in the output of gcloud compute instances list, you will see both of the virtual machines you created.

Look at the External IP column. Notice that the external IP address of the first VM you created is shown as a link. (If necessary, click the HIDE INFO PANEL button to reveal the External IP column.) The Google Cloud Console offers the link because you configured this VM's firewall to allow HTTP traffic.

Click the link you found in your VM's External IP column. Your browser will present a Connection refused message in a new browser tab. This message occurs because, although there is a firewall port open for HTTP traffic to your VM, no Web server is running there. Close the browser tab you just created.

Use the gcloud command line to create a second service account
In Cloud Shell, execute the following command to create a new service account:

gcloud iam service-accounts create test-service-account2 --display-name "test-service-account2"
If you see the following output, type y and press ENTER:

Output (do not copy)

API [iam.googleapis.com] not enabled on project [560255523887]. Would
you like to enable and retry (this will take a few minutes)? (y/N)?
In the Google Cloud Console, on the Navigation menu (Navigation menu), click IAM & admin > Service accounts.
Refresh the page till you see test-service-account2.

Click Check my progress to verify the objective.
Create a second bucket, VM instance and an IAM service account.

In Cloud Shell, execute the following command to grant the second service account the Project viewer role:

gcloud projects add-iam-policy-binding $GOOGLE_CLOUD_PROJECT --member serviceAccount:test-service-account2@${GOOGLE_CLOUD_PROJECT}.iam.gserviceaccount.com --role roles/viewer
GOOGLE_CLOUD_PROJECT is an environment variable that is automatically populated in Cloud Shell and is set to the project ID of the current context.

In the Google Cloud Console, on the Navigation menu (Navigation menu), click IAM & admin > IAM. Select the new service account called test-service-account2.
On the right hand side of the page, click on pencil icon and expand the Viewer role.
You will see test-service-account2 listed as a member of the Viewer role.

Task 3. Work with Cloud Storage in Cloud Shell
Download a file to Cloud Shell and copy it to Cloud Storage
Copy a picture of a cat from a Google-provided Cloud Storage bucket to your Cloud Shell.

gsutil cp gs://cloud-training/ak8s/cat.jpg cat.jpg
Copy the file into one of the buckets that you created earlier.

gsutil cp cat.jpg gs://$MY_BUCKET_NAME_1
Copy the file from the first bucket into the second bucket:

gsutil cp gs://$MY_BUCKET_NAME_1/cat.jpg gs://$MY_BUCKET_NAME_2/cat.jpg
In the Google Cloud Console, on the Navigation menu (Navigation menu), click Storage > Browser, select the buckets that you created, and verify that both contain the cat.jpg file.

Set the access control list for a Cloud Storage object
To get the default access list that's been assigned to cat.jpg (when you uploaded it to your Cloud Storage bucket), execute the following two commands:

gsutil acl get gs://$MY_BUCKET_NAME_1/cat.jpg  > acl.txt
cat acl.txt
The output should look like the following example, but with different numbers. This output shows that anyone with a Project Owner, Editor, or Viewer role for the project has access (Owner access for Owners/Editors and Reader access for Viewers).

Output (do not copy)

[
  {
    "entity": "project-owners-560255523887",
    "projectTeam": {
      "projectNumber": "560255523887",
      "team": "owners"
    },
gsutil cp gs://cloud-training/ak8s/cat.jpg cat.jpg    "role": "OWNER"
  },
  {
    "entity": "project-editors-560255523887",
    "projectTeam": {
      "projectNumber": "560255523887",
      "team": "editors"
    },
    "role": "OWNER"
  },
  {
    "entity": "project-viewers-560255523887",
    "projectTeam": {
      "projectNumber": "560255523887",
      "team": "viewers"
    },
    "role": "READER"
  },
  {
    "email": "google12345678_student@qwiklabs.net",
    "entity": "user-google12345678_student@qwiklabs.net",
    "role": "OWNER"
  }
]
To change the object to have private access, execute the following command:

gsutil acl set private gs://$MY_BUCKET_NAME_1/cat.jpg
To verify the new ACL that's been assigned to cat.jpg, execute the following two commands:

gsutil acl get gs://$MY_BUCKET_NAME_1/cat.jpg  > acl-2.txt
cat acl-2.txt
The output should look similar to the following example. Now only the original creator of the object (your lab account) has Owner access.

Output (do not copy)

[
  {
    "email": "google12345678_student@qwiklabs.net",
    "entity": "user-google12345678_student@qwiklabs.net",
    "role": "OWNER"
  }
]
Authenticate as a service account in Cloud Shell
In Cloud Shell, execute the following command to view the current configuration:

gcloud config list
You should see output that looks like the following example. In your output, the zone should be equal to the zone that you set when you created your second VM in task 2. The account and project should match your Qwiklabs lab credentials.

Output (do not copy)

[component_manager]
disable_update_check = True
[compute]
gce_metadata_read_timeout_sec = 5
zone = us-central1-a
[core]
account = google12345678_student@qwiklabs.net
disable_usage_reporting = False
project = qwiklabs-Google Cloud-1aeffbc5d0acb416
[metrics]
environment = devshell

Your active configuration is: [cloudshell-16441]
In Cloud Shell, execute the following command to change the authenticated user to the first service account (which you created in an earlier task) through the credentials that you downloaded to your local machine and then uploaded into Cloud Shell (credentials.json).

gcloud auth activate-service-account --key-file credentials.json
Cloud Shell is now authenticated as test-service-account.

To verify the active account, execute the following command:

gcloud config list
You should see output that looks like the following example. The account is now set to the test-service-account service account.

Output (do not copy)

[component_manager]
disable_update_check = True
[compute]
gce_metadata_read_timeout_sec = 5
zone = us-central1-a
[core]
account = test-service-account@qwiklabs-Google Cloud-1aeffbc5d0acb416.iam.gserviceaccount.com
disable_usage_reporting = False
project = qwiklabs-Google Cloud-1aeffbc5d0acb416
[metrics]
environment = devshell

Your active configuration is: [cloudshell-16441]
To verify the list of authorized accounts in Cloud Shell, execute the following command:

gcloud auth list
You should see output that looks like the following example.

Output (do not copy)

                Credentialed Accounts
ACTIVE  ACCOUNT
        google12345678_student@qwiklabs.net
*       test-service-account@qwiklabs-Google Cloud-1aeffbc5d0acb416.iam.gserviceaccount.com

To set the active account, run:
    $ gcloud config set account `ACCOUNT`
To verify that the current account (test-service-account) cannot access the cat.jpg file in the first bucket that you created, execute the following command:

gsutil cp gs://$MY_BUCKET_NAME_1/cat.jpg ./cat-copy.jpg
Because you restricted access to this file to the owner earlier in this task you should see output that looks like the following example.

Output (do not copy)

Copying gs://test-bucket-123/cat.jpg...
AccessDeniedException: 403  KiB]
Verify that the current account (test-service-account) can access the cat.jpg file in the second bucket that you created:

gsutil cp gs://$MY_BUCKET_NAME_2/cat.jpg ./cat-copy.jpg
Because access has not been restricted to this file you should see output that looks like the following example.

Output (do not copy)

Copying gs://test-bucket-123/cat.jpg...
- [1 files][ 81.7 KiB/ 81.7 KiB]
Operation completed over 1 objects/81.7 KiB.
To switch to the lab account, execute the following command.
You replace [USERNAME] with the username provided in the Qwiklabs Connection Details pane on the left of the lab instructions page. .

gcloud config set account [USERNAME]
To verify that you can access the cat.jpg file in the [BUCKET_NAME] bucket (the first bucket that you created), execute the following command.

gsutil cp gs://$MY_BUCKET_NAME_1/cat.jpg ./copy2-of-cat.jpg
You should see output that looks like the following example. The lab account created the bucket and object and remained an Owner when the object access control list (ACL) was converted to private, so the lab account can still access the object.

Output (do not copy)

Copying gs://test-bucket-123/cat.jpg...
- [1 files][ 81.7 KiB/ 81.7 KiB]
Operation completed over 1 objects/81.7 KiB.
Make the first Cloud Storage bucket readable by everyone, including unauthenticated users.

gsutil iam ch allUsers:objectViewer gs://$MY_BUCKET_NAME_1
This is an appropriate setting for hosting public website content in Cloud Storage.

In the Google Cloud Console, on the Navigation menu (Navigation menu), click Storage > Browser, select the first storage bucket that you created. Notice that the cat.jpg file has a Public link. Copy this link.
Open an incognito browser tab and paste the link into its address bar. You will see a picture of a cat. Leave this browser tab open.
Click Check my progress to verify the objective.
Work with the Cloud Storage in Cloud Shell.

Task 4. Explore the Cloud Shell code editor
In this task, you explore using the Cloud Shell code editor.

Open the Cloud Shell code editor
In Cloud Shell, click the Open in new window icon on the top right. Then click the pencil icon to open the Cloud Shell code editor.
Cloud Shell Code Editor icon

A new tab opens with the Cloud Shell Code editor and the Cloud Shell. The Google Cloud console remains on the original tab. You can switch between the Google Cloud Console and Cloud Shell by clicking the tab.

In Cloud Shell, execute the following command to clone a git repository:

git clone https://github.com/googlecodelabs/orchestrate-with-kubernetes.git
The orchestrate-with-kubernetes folder appears in the left pane of the Cloud Shell code editor window.

File-refresh menu

In Cloud Shell, execute the following command to create a test directory:

mkdir test
The test folder now appears in the left pane of the Cloud Shell code editor window.

test-folder-revealed

In the Cloud Shell code editor, click the arrow to the left of orchestrate-with-kubernetes to expand the folder.
expand-folder

Click the cleanup.sh file to open it in the right pane of the Cloud Shell code editor window.
cleanup

Add the following text as the last line of the cleanup.sh file:

echo Finished cleanup!
No action is necessary to save your work.

In Cloud Shell, execute the following commands to change directory and display the contents of cleanup.sh:

cd orchestrate-with-kubernetes
cat cleanup.sh
Verify that the output of cat cleanup.sh includes the line of text that you added.

In the Cloud Shell code editor, click to open the File menu and choose New File. Name the file index.html.

In the right hand pane, paste in this HTML text:

<html><head><title>Cat</title></head>
<body>
<h1>Cat</h1>
<img src="REPLACE_WITH_CAT_URL">
</body></html>
Use your local computer's keyboard shortcut to paste: `Cmd-V` for a Mac, `Ctrl-V` for a Windows or Linux machine.

Replace the string REPLACE_WITH_CAT_URL with the URL of the cat image from an earlier task. The URL will look like this:

![source](img/assets/cat.jpg)
(https://storage.googleapis.com/qwiklabs-gcp-01-79c0b5cf7db1/cat.jpg)

Eg. https://storage.googleapis.com/qwiklabs-gcp-01-79c0b5cf7db1/cat.jpg
On the Navigation menu (Navigation menu ), click Compute Engine > VM instances.

In the row for your first VM, click the SSH button.

In the SSH login window that opens on your VM, install the nginx Web server:

sudo apt-get update
sudo apt-get install nginx
It may take few minutes to complete the process.
In your Cloud Shell window, copy the HTML file you created using the Code Editor to your virtual machine:

gcloud compute scp index.html first-vm:index.nginx-debian.html --zone=us-central1-c
If you are prompted whether to add a host key to your list of known hosts, answer y.

If you are prompted to enter a passphrase, press the Enter key to respond with an empty passphrase. Press the Enter key again when prompted to confirm the empty passphrase.

In the SSH login window for your VM, copy the HTML file from your home directory to the document root of the nginx Web server:

sudo cp index.nginx-debian.html /var/www/html
Click Check my progress to verify the objective.
Install the nginx Web server and customize the welcome page.

On the Navigation menu (Navigation menu ), click Compute Engine > VM instances. Click the link in the External IP column for your first VM. A new browser tab opens, containing a Web page that contains the cat image


### Note.
Some steps were missing, such as `cd orchestrate-with-kubernetes` and "stop SSH window and enter `sudo cp index.nginx-debian.html /var/www/html` in a new one.

***


## Lab2: Working with Cloud Build
1 hour
Free
Rate Lab
Overview
In this lab you will build a Docker container image from provided code and a Dockerfile using Cloud Build. You will then upload the container to Container Registry.

Objectives
In this lab, you learn how to perform the following tasks:

Use Cloud Build to build and push containers

Use Container Registry to store and deploy containers

Task 0. Lab Setup
Access Qwiklabs
For each lab, you get a new GCP project and set of resources for a fixed time at no cost.

Make sure you signed into Qwiklabs using an incognito window.

Note the lab's access time (for example, img/time.png and make sure you can finish in that time block.

There is no pause feature. You can restart if needed, but you have to start at the beginning.

When ready, click img/start_lab.png.

Note your lab credentials. You will use them to sign in to Cloud Platform Console. img/open_google_console.png

Click Open Google Console.

Click Use another account and copy/paste credentials for this lab into the prompts.

If you use other credentials, you'll get errors or incur charges.

Accept the terms and skip the recovery resource page.
Do not click End Lab unless you are finished with the lab or want to restart it. This clears your work and removes the project.

After you complete the initial sign-in steps, the project dashboard appears.

c679b27aa332f22d.png

Task 1: Confirm that needed APIs are enabled
Make a note of the name of your Google Cloud project. This value is shown in the top bar of the Google Cloud Console. It will be of the form qwiklabs-gcp- followed by hexadecimal numbers.

In the Google Cloud Console, on the Navigation menu(Navigation menu), click APIs & Services.

Click Enable APIs and Services.

In the Search for APIs & Services box, enter Cloud Build.

In the resulting card for the Cloud Build API, if you do not see a message confirming that the Cloud Build API is enabled, click the ENABLE button.

Use the Back button to return to the previous screen with a search box. In the search box, enter Container Registry.

In the resulting card for the Container Registry API, if you do not see a message confirming that the Container Registry API is enabled, click the ENABLE button.

Task 2. Building Containers with DockerFile and Cloud Build
You can write build configuration files to provide instructions to Cloud Build as to which tasks to perform when building a container. These build files can fetch dependencies, run unit tests, analyses and more. In this task, you'll create a DockerFile and use it as a build configuration script with Cloud Build. You will also create a simple shell script (quickstart.sh) which will represent an application inside the container.

On the Google Cloud Console title bar, click Activate Cloud Shell.

When prompted, click Continue.

Cloud Shell opens at the bottom of the Google Cloud Console window.

Create an empty quickstart.sh file using the nano text editor.

nano quickstart.sh
Add the following lines in to the quickstart.sh file:

#!/bin/sh
echo "Hello, world! The time is $(date)."
Save the file and close nano by pressing the CTRL+X key, then press Y and Enter.

Create an empty Dockerfile file using the nano text editor.

nano Dockerfile
Add the following Dockerfile command:

FROM alpine
This instructs the build to use the Alpine Linux base image.

Add the following Dockerfile command to the end of the Dockerfile:

COPY quickstart.sh /
This adds the quickstart.sh script to the / directory in the image.

Add the following Dockerfile command to the end of the Dockerfile:

CMD ["/quickstart.sh"]
This configures the image to execute the /quickstart.sh script when the associated container is created and run.

The Dockerfile should now look like:

FROM alpine
COPY quickstart.sh /
CMD ["/quickstart.sh"]
Save the file and close nano by pressing the CTRL+X key, then press Y and Enter.

In Cloud Shell, run the following command to make the quickstart.sh script executable.

chmod +x quickstart.sh
In Cloud Shell, run the following command to build the Docker container image in Cloud Build.

gcloud builds submit --tag gcr.io/${GOOGLE_CLOUD_PROJECT}/quickstart-image .
Important

Don't miss the dot (".") at the end of the command. The dot specifies that the source code is in the current working directory at build time.

When the build completes, your Docker image is built and pushed to Container Registry.

In the Google Cloud Console, on the Navigation menu (Navigation menu), click Container Registry > Images.
5ea63873c5756db4.png

The quickstart-image Docker image appears in the list

Task 3. Building Containers with a build configuration file and Cloud Build
Cloud Build also supports custom build configuration files. In this task you will incorporate an existing Docker container using a custom YAML-formatted build file with Cloud Build.

In Cloud Shell enter the following command to clone the repository to the lab Cloud Shell.

git clone https://github.com/GoogleCloudPlatform/training-data-analyst
Create a soft link as a shortcut to the working directory.

ln -s ~/training-data-analyst/courses/ak8s/v1.1 ~/ak8s
Change to the directory that contains the sample files for this lab.

cd ~/ak8s/Cloud_Build/a
A sample custom cloud build configuration file called cloudbuild.yaml has been provided for you in this directory as well as copies of the Dockerfile and the quickstart.sh script you created in the first task.

In Cloud Shell, execute the following command to view the contents of cloudbuild.yaml.

cat cloudbuild.yaml
You will see the following:

steps:
- name: 'gcr.io/cloud-builders/docker'
  args: [ 'build', '-t', 'gcr.io/$PROJECT_ID/quickstart-image', '.' ]
images:
- 'gcr.io/$PROJECT_ID/quickstart-image'
This file instructs Cloud Build to use Docker to build an image using the Dockerfile specification in the current local directory, tag it with gcr.io/$PROJECT_ID/quickstart-image ($PROJECT_ID is a substitution variable automatically populated by Cloud Build with the project ID of the associated project) and then push that image to Container Registry.

In Cloud Shell, execute the following command to start a Cloud Build using cloudbuild.yaml as the build configuration file:

gcloud builds submit --config cloudbuild.yaml .
The build output to Cloud Shell should be the same as before. When the build completes, a new version of the same image is pushed to Container Registry.

In the Google Cloud Console, on the Navigation menu (Navigation menu), click Container Registry > Images and then click quickstart-image.
20760c66fe33e5c7.png

Two versions of quickstart-image are now in the list.

Click Check my progress to verify the objective.
Build two Container images in Cloud Build.

In the Google Cloud Console, on the Navigation menu (Navigation menu), click Cloud Build > History.
build1.png

Two builds appear in the list.

Click the build ID for the build at the top of the list.
build2.png

The details of the build, including the build log, are displayed.

Task 4. Building and Testing Containers with a build configuration file and Cloud Build
The true power of custom build configuration files is their ability to perform other actions, in parallel or in sequence, in addition to simply building containers: running tests on your newly built containers, pushing them to various destinations, and even deploying them to Kubernetes Engine. In this lab, we will see a simple example: a build configuration file that tests the container it built and reports the result to its calling environment.

In Cloud Shell, change to the directory that contains the sample files for this lab.

cd ~/ak8s/Cloud_Build/b
As before, the quickstart.sh script and the a sample custom cloud build configuration file called cloudbuild.yaml has been provided for you in this directory. These have been slightly modified to demonstrate Cloud Build's ability to test the containers it has build. There is also a Dockerfile present, which is identical to the one used for the previous task.

In Cloud Shell, execute the following command to view the contents of cloudbuild.yaml.

cat cloudbuild.yaml
You will see the following:

steps:
- name: 'gcr.io/cloud-builders/docker'
  args: [ 'build', '-t', 'gcr.io/$PROJECT_ID/quickstart-image', '.' ]
- name: 'gcr.io/$PROJECT_ID/quickstart-image'
  args: ['fail']
images:
- 'gcr.io/$PROJECT_ID/quickstart-image
In addition to its previous actions, this build configuration file runs the quickstart-image it has created. In this task, the quickstart.sh script has been modified so that it simulates a test failure when an argument ['fail'] is passed to it.

In Cloud Shell, execute the following command to start a Cloud Build using cloudbuild.yaml as the build configuration file:

gcloud builds submit --config cloudbuild.yaml .
You will see output from the command that ends with text like this:

Output (do not copy)

Finished Step #1
ERROR
ERROR: build step 1 "gcr.io/ivil-charmer-227922klabs-gcp-49ab2930eea05/quickstart-image" failed: exit status 127
----------------------------------------------------------------------------------------------------------------------------------------------------------------
ERROR: (gcloud.builds.submit) build f3e94c28-fba4-4012-a419-48e90fca7491 completed with status "FAILURE"
Confirm that your command shell knows that the build failed:

echo $?
The command will reply with a non-zero value. If you had embedded this build in a script, your script would be able to act up on the build's failure.


![print screen gbuild failure](img/assets/lab2_gcloud-build.png)

![print screen gbuild shell ](img/assets/lab2_gcloud-shell.png)


Click Check my progress to verify the objective.
Build and Test Containers with a build configuration file and Cloud Build



### Lab3: Deploying Google Kubernetes Engine
1 hour
Free
Rate Lab
Overview
In this lab, you use the Google Cloud Console to build GKE clusters and deploy a sample Pod.

Objectives
In this lab, you learn how to perform the following tasks:

Use the Google Cloud Console to build and manipulate GKE clusters

Use the Google Cloud Console to deploy a Pod

Use the Google Cloud Console to examine the cluster and Pods

Task 0. Lab Setup
Access Qwiklabs
For each lab, you get a new GCP project and set of resources for a fixed time at no cost.

Make sure you signed into Qwiklabs using an incognito window.

Note the lab's access time (for example, img/time.png and make sure you can finish in that time block.

There is no pause feature. You can restart if needed, but you have to start at the beginning.

When ready, click img/start_lab.png.

Note your lab credentials. You will use them to sign in to Cloud Platform Console. img/open_google_console.png

Click Open Google Console.

Click Use another account and copy/paste credentials for this lab into the prompts.

If you use other credentials, you'll get errors or incur charges.

Accept the terms and skip the recovery resource page.
Do not click End Lab unless you are finished with the lab or want to restart it. This clears your work and removes the project.

After you complete the initial sign-in steps, the project dashboard appears.

c679b27aa332f22d.png

Task 1. Deploy GKE clusters
In this task, you use the Google Cloud Console and Cloud Shell to deploy GKE clusters.

Use the Google Cloud Console to deploy a GKE cluster
In the Google Cloud Console, on the Navigation menu (9a951fa6d60a98a5.png), click Kubernetes Engine > Clusters.
82b6a27127bebd94.png

Click Create cluster to begin creating a GKE cluster.

Examine the console UI and the controls to change the cluster name, the cluster location, Kubernetes version, the number of nodes, and the node resources such as the machine type in the default node pool.

Clusters can be created across a region or in a single zone. A single zone is the default. When you deploy across a region the nodes are deployed to three separate zones and the total number of nodes deployed will be three times higher.

Change the cluster name to standard-cluster-1 and zone to us-central1-a. Leave all the values at their defaults and click Create.
The cluster begins provisioning.

Note: You need to wait a few minutes for the cluster deployment to complete.
When provisioning is complete, the Kubernetes Engine > Clusters page looks like the screenshot:

f46327c053cbb49b.png

Click Check my progress to verify the objective.
Deploy GKE cluster

Click the cluster name standard-cluster-1 to view the cluster details
6a505cd98a4e854e.png

You can scroll down the page to view more details.

Click the Storage and Nodes tabs under the cluster name (standard-cluster-1) at the top to view more of the cluster details.

Task 2. Modify GKE clusters
It is easy to modify many of the parameters of existing clusters using either the Google Cloud Console or Cloud Shell. In this task, you use the Google Cloud Console to modify the size of GKE clusters.

In the Google Cloud Console, click Edit at the top of the details page for standard-cluster-1.
Scroll down to the Node Pools section and click default pool.
In the Google Cloud Console, click Edit at the top of the details page.
In the Size section, change the number of nodes from 3 to 4.
changenode.png

Scroll to the bottom and click Save.
In the Google Cloud Console, on the Navigation menu (9a951fa6d60a98a5.png), click Kubernetes Engine > Clusters.
When the operation completes, the Kubernetes Engine > Clusters page should show that standard-cluster-1 now has four nodes.

110af9591326966.png

Click Check my progress to verify the objective.
Modify GKE clusters

Task 3. Deploy a sample workload
In this task, using the Google Cloud console you will deploy a Pod running the nginx web server as a sample workload.

In the Google Cloud Console, on the Navigation menu( Navigation menu), click Kubernetes Engine > Workloads.
Click Deploy to show the Create a deployment wizard.
b5ff57c1a950cc70.png

Click Continue to accept the default container image, nginx.latest, which deploys a Pod with a single container running the latest version of nginx.
15d282aa7690eacf.png

Scroll to the bottom of the window and click the Deploy button leaving the Configuration details at the defaults.
When the deployment completes your screen will refresh to show the details of your new nginx deployment.
a4a2ee9873a2dc2f.png

Click Check my progress to verify the objective.
Deploy a sample nginx workload

Task 4. View details about workloads in the Google Cloud Console
In this task, you view details of your GKE workloads directly in the Google Cloud Console.

In the Google Cloud Console, on the Navigation menu (Navigation menu), click Kubernetes Engine > Workloads.
32a95eb42719841a.png

In the Google Cloud Console, on the Kubernetes Engine > Workloads page, click nginx-1.
You may see Pods (3/3) as the default deployment will start with three pods but will scale back to 1 after a few minutes. You can continue with the lab.
This displays the overview information for the workload showing details like resource utilization charts, links to logs, and details of the Pods associated with this workload.

871369f92b245567.png

In the Google Cloud Console, click the Details tab for the nginx-1 workload. The Details tab shows more details about the workload including the Pod specification, number and status of Pod replicas and details about the horizontal Pod autoscaler.
b71b95bf5d1ff3e5.png

Click the Revision History tab. This displays a list of the revisions that have been made to this workload.
b2532b5efc81c672.png

Click the Events tab. This tab lists events associated with this workload.
2a770ffe99a8ae05.png

And then the YAML tab. This tab provides the complete YAML file that defines this components and full configuration of this sample workload.
1f074ceacbc117f4.png

Still in the Google Cloud Console's Details tab for the nginx-1 workload, click the Overview tab, scroll down to the Managed Pods section and click the name of one of the Pods to view the details page for that Pod.
4183f157b2afea06.png

Note:

The default deployment will start with three pods but will scale back to 1 after a few minutes so you may need to refresh the Overview page to make sure you have a valid Pod to inspect.

The Pod Details page provides information on the Pod configuration and resource utilization and the node where the Pod is running.
1c7955b9e33f3f5b.png

In the Pod details page, you can click the Events and Logs tabs to view event details and links to container logs in Cloud Operations.
event.png

logs.png

Click the YAML tab to view the detailed YAML file for the Pod configuration.
a3174a5f4dcf8b68.png


***

## Lab3: Deploying Google Kubernetes Engine
1 hour
Free
Rate Lab
Overview
In this lab, you use the Google Cloud Console to build GKE clusters and deploy a sample Pod.

Objectives
In this lab, you learn how to perform the following tasks:

Use the Google Cloud Console to build and manipulate GKE clusters

Use the Google Cloud Console to deploy a Pod

Use the Google Cloud Console to examine the cluster and Pods

Task 0. Lab Setup
Access Qwiklabs
For each lab, you get a new GCP project and set of resources for a fixed time at no cost.

Make sure you signed into Qwiklabs using an incognito window.

Note the lab's access time (for example, img/time.png and make sure you can finish in that time block.

There is no pause feature. You can restart if needed, but you have to start at the beginning.

When ready, click img/start_lab.png.

Note your lab credentials. You will use them to sign in to Cloud Platform Console. img/open_google_console.png

Click Open Google Console.

Click Use another account and copy/paste credentials for this lab into the prompts.

If you use other credentials, you'll get errors or incur charges.

Accept the terms and skip the recovery resource page.
Do not click End Lab unless you are finished with the lab or want to restart it. This clears your work and removes the project.

After you complete the initial sign-in steps, the project dashboard appears.

c679b27aa332f22d.png

Task 1. Deploy GKE clusters
In this task, you use the Google Cloud Console and Cloud Shell to deploy GKE clusters.

Use the Google Cloud Console to deploy a GKE cluster
In the Google Cloud Console, on the Navigation menu (9a951fa6d60a98a5.png), click Kubernetes Engine > Clusters.
82b6a27127bebd94.png

Click Create cluster to begin creating a GKE cluster.

Examine the console UI and the controls to change the cluster name, the cluster location, Kubernetes version, the number of nodes, and the node resources such as the machine type in the default node pool.

Clusters can be created across a region or in a single zone. A single zone is the default. When you deploy across a region the nodes are deployed to three separate zones and the total number of nodes deployed will be three times higher.

Change the cluster name to standard-cluster-1 and zone to us-central1-a. Leave all the values at their defaults and click Create.
The cluster begins provisioning.

Note: You need to wait a few minutes for the cluster deployment to complete.
When provisioning is complete, the Kubernetes Engine > Clusters page looks like the screenshot:

f46327c053cbb49b.png

Click Check my progress to verify the objective.
Deploy GKE cluster

Click the cluster name standard-cluster-1 to view the cluster details
6a505cd98a4e854e.png

You can scroll down the page to view more details.

Click the Storage and Nodes tabs under the cluster name (standard-cluster-1) at the top to view more of the cluster details.

Task 2. Modify GKE clusters
It is easy to modify many of the parameters of existing clusters using either the Google Cloud Console or Cloud Shell. In this task, you use the Google Cloud Console to modify the size of GKE clusters.

In the Google Cloud Console, click Edit at the top of the details page for standard-cluster-1.
Scroll down to the Node Pools section and click default pool.
In the Google Cloud Console, click Edit at the top of the details page.
In the Size section, change the number of nodes from 3 to 4.
changenode.png

Scroll to the bottom and click Save.
In the Google Cloud Console, on the Navigation menu (9a951fa6d60a98a5.png), click Kubernetes Engine > Clusters.
When the operation completes, the Kubernetes Engine > Clusters page should show that standard-cluster-1 now has four nodes.

110af9591326966.png

Click Check my progress to verify the objective.
Modify GKE clusters

Task 3. Deploy a sample workload
In this task, using the Google Cloud console you will deploy a Pod running the nginx web server as a sample workload.

In the Google Cloud Console, on the Navigation menu( Navigation menu), click Kubernetes Engine > Workloads.
Click Deploy to show the Create a deployment wizard.
b5ff57c1a950cc70.png

Click Continue to accept the default container image, nginx.latest, which deploys a Pod with a single container running the latest version of nginx.
15d282aa7690eacf.png

Scroll to the bottom of the window and click the Deploy button leaving the Configuration details at the defaults.
When the deployment completes your screen will refresh to show the details of your new nginx deployment.
a4a2ee9873a2dc2f.png

Click Check my progress to verify the objective.
Deploy a sample nginx workload

Task 4. View details about workloads in the Google Cloud Console
In this task, you view details of your GKE workloads directly in the Google Cloud Console.

In the Google Cloud Console, on the Navigation menu (Navigation menu), click Kubernetes Engine > Workloads.
32a95eb42719841a.png

In the Google Cloud Console, on the Kubernetes Engine > Workloads page, click nginx-1.
You may see Pods (3/3) as the default deployment will start with three pods but will scale back to 1 after a few minutes. You can continue with the lab.
This displays the overview information for the workload showing details like resource utilization charts, links to logs, and details of the Pods associated with this workload.

871369f92b245567.png

In the Google Cloud Console, click the Details tab for the nginx-1 workload. The Details tab shows more details about the workload including the Pod specification, number and status of Pod replicas and details about the horizontal Pod autoscaler.
b71b95bf5d1ff3e5.png

Click the Revision History tab. This displays a list of the revisions that have been made to this workload.
b2532b5efc81c672.png

Click the Events tab. This tab lists events associated with this workload.
2a770ffe99a8ae05.png

And then the YAML tab. This tab provides the complete YAML file that defines this components and full configuration of this sample workload.
1f074ceacbc117f4.png

Still in the Google Cloud Console's Details tab for the nginx-1 workload, click the Overview tab, scroll down to the Managed Pods section and click the name of one of the Pods to view the details page for that Pod.
4183f157b2afea06.png

Note:

The default deployment will start with three pods but will scale back to 1 after a few minutes so you may need to refresh the Overview page to make sure you have a valid Pod to inspect.

The Pod Details page provides information on the Pod configuration and resource utilization and the node where the Pod is running.
1c7955b9e33f3f5b.png

In the Pod details page, you can click the Events and Logs tabs to view event details and links to container logs in Cloud Operations.
event.png

logs.png

Click the YAML tab to view the detailed YAML file for the Pod configuration.
a3174a5f4dcf8b68.png

![yaml file](img/assets/lab3_yaml.png)















