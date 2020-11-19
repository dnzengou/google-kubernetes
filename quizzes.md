#

## Kubernetes Architecture
### Architecting with Google Kubernetes Engine: Foundations

TOTAL DES POINTS 7
1.
Question 1
You are designing an application, and you want to ensure that the containers are located as close to each other as possible, in order to minimize latency. Which design decision helps meet this requirement?

1 point

Place the containers in the same cluster.


Place the containers in the same Namespace.


Give the containers the same labels.


Place the containers in the same Pod.

2.
Question 2
Which Kubernetes component does the kubectl command connect to in order to carry out operations on a cluster?

1 point

kube-controller-manager


kube-scheduler


kube-apiserver


kube-dns

3.
Question 3
You have deployed a new Kubernetes Engine regional cluster with four machines in the default pool for the first zone and left the number of zones at the default. How many Compute Engine machines are deployed and billed against your account?

1 point

Ten. (Four nodes are deployed in the first zone and three nodes are deployed in two other zones because you selected the defaults.)


Twelve. (Four nodes are deployed in each of three zones. A master node is deployed in each zone but it is not billed against your account.)


Fifteen. (Four nodes and a single master are deployed to each of the three zones. A master node is deployed in each zone and it is billed against your account.)


Sixteen. (Four nodes are deployed in primary and secondary zones in two regions, for a total of 4 zones and 16 nodes. A master node is deployed in each zone but it is not billed to your account.)

4.
Question 4
You need to ensure that the production applications running on your Kubernetes cluster are not impacted by test and staging deployments. Which features should you implement and configure to ensure that the resources for your production applications can be prioritized?

1 point

Configure resource requests for Test, Staging and Production and configure specific Kubernetes resource quotas for the Production Namespace.


Configure Namespaces for Test, Staging and Production and configure specific Kubernetes resource quotas for the Production Namespace.


Configure labels for Test, Staging and Production and configure specific Kubernetes resource quotas for the Production Namespace.


Configure Namespaces for Test, Staging and Production and configure specific Kubernetes resource quotas for the test and staging Namespaces.

5.
Question 5
When configuring storage for stateful applications, what steps must you take to provide file system storage inside your containers for data from your applications that will not be lost or deleted if your Pods fail or are deleted for any reason?

1 point

You must mount NFS Volumes on each container in the Pod that requires durable storage.


You must export the data from your applications to a remote service that preserves your data.


You must create Volumes using local Storage on the Nodes and mount the Volumes inside your containers to provide durable storage.


You must create Volumes using network based storage to provide durable storage remote to the Pods and specify these in the Pods.

6.
Question 6
You have a new logging and auditing utility that you need to deploy on all of the nodes within your cluster. Which type of controller should you use to handle this task?

1 point

StatefulSet


ReplicaSet


DaemonSet


Deployment.

7.
Question 7
You want to deploy multiple copies of your application, so that you can load balance traffic across them. How should you deploy this application's Pods to the production Namespace in your cluster?

1 point

Deploy the Pod manifest multiple times until you have achieved the number of replicas required.


Create a Deployment manifest that specifies the number of replicas that you want to run.


Create separate named Pod manifests for each instance of the application and deploy as many as you need.


Create a Service manifest for the LoadBalancer that specifies the number of replicas you want to run.

