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
    

**Creating a Kubernetes cluster**

- Create a Compute Engine network for the Container Engine cluster to connect to and use

    ```gcloud compute networks create jenkins --mode auto```

- Provision a Kubernetes cluster using Container Engine

    ```gcloud container clusters create jenkins-cd \
        --network jenkins \
        --scopes "https://www.googleapis.com/auth/projecthosting,storage-rw"```

- Confirm that your cluster is running.

    ``` gcloud container clusters list ```
    
- Get the credentials for your cluster. Container Engine uses these credentials to access your newly provisioned cluster.

    ```gcloud container clusters get-credentials jenkins-cd```

- Confirm that you can connect to your cluster.

    ```kubectl cluster-info```
    
**Creating the Jenkins home volume**


    ```gcloud compute images create jenkins-home-image --source-uri https://storage.googleapis.com/solutions-public- assets/jenkins-cd/jenkins-home-v3.tar.gz```

    ```gcloud compute disks create jenkins-home --image jenkins-home-image --zone us-east1-d```

- Configuring Jenkins credentials

- Open the jenkins/k8s/options file, and replace CHANGE_ME with a new password.

- Save and exit the file.

    ```nano jenkins/k8s/options ```


- create a Kubernetes namespace for Jenkins.

    ```kubectl create ns jenkins```

- create a Kubernetes secret. Kubernetes uses this object to provide Jenkins with the default username and password when Jenkins boots.

    ```kubectl create secret generic jenkins --from-file=jenkins/k8s/options --namespace=jenkins```

- Creating the Jenkins deployment and services

    ```kubectl apply -f jenkins/k8s/```

- Confirm that the pod is running.

    ```kubectl get pods --namespace jenkins```

**Configuring external load balancing**

- List the Jenkins services.

    ```kubectl get svc --namespace jenkins```

**Setting up encryption**

- If you don’t already have an SSL certificate for your domain, you can create a temporary SSL certificate and key pair by running the following command.



    ```openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /tmp/tls.key -out /tmp/tls.crt -subj         "/CN=jenkins/O=jenkins"````

- Upload the certificate to Kubernetes as a secret object

    ```kubectl create secret generic tls --from-file=/tmp/tls.crt --from-file=/tmp/tls.key --namespace jenkins```

- Creating the load balancer

    ```kubectl apply -f jenkins/k8s/lb/ingress.yaml```

**Connecting to Jenkins**

- Check the status of the load balancer’s health checks.

    ```kubectl describe ingress jenkins --namespace jenkins````

-  copy ip address from the output of previous command and open in browser

- Log in with Jenkins credentials
    
    uname: Jenkins
    
    pwd: same as you set up previously
