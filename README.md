# DevOps_Techtalk

# Google cloud platform

## Setting up Jenkins on Container Engine


**Objectives**

- Creating a Kubernetes cluster with Container Engine.

- Creating a Jenkins deployment and services.

- Configuring external load balancing.

- Connecting to Jenkins.



**Prerequisites:**

- Set up a GCP account

- Enable billing

- Enable the Google Compute Engine, Google Container Engine APIs.



**Set up and Steps:**

- Activate google cloud shell 

- Set the default Compute Engine zone to us-east1-d

    ``` gcloud config set compute/zone us-east1-d ```
    
- Clone the sample code
    
    ```git clone https://github.com/GoogleCloudPlatform/continuous-deployment-on-kubernetes.git```
    
- Navigate to the sample code directory

    ```cd continuous-deployment-on-kubernetes```
