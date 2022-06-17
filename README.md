# Creating CI/CD Pipeline with GCP

If we look at the landscape of cloud, one thing it has undoubtedly revolutionized is how we design and deploy our applications. From the large monoliths just a decade ago, to well-containerized and scalable solutions, we have come a long way. Google has a nice [comic strip](https://cloud.google.com/kubernetes-engine/kubernetes-comic) describing this journey and the reasons for it. Still a lot of time continues to be spent on analyzing, building, and debugging this pipeline. To save some of that and for quick prototyping of applications deployed on Kubernetes on GCP, the following reference architecture can be used and adapted to the specific needs of a project.


## Overview

![Deployment Diagram](https://github.com/itsamk/gcp-application-pipeline/blob/main/assets/gcp-cd.png)
The above diagram represents various cloud components that are used in this pipeline. Here are the links to those for more information –

Kubernetes - https://cloud.google.com/kubernetes-engine

Cloud Load Balancing - https://cloud.google.com/load-balancing

Managed Certificate - https://cloud.google.com/load-balancing/docs/ssl-certificates/google-managed-certs

DNS Server - https://www.cloudflare.com/en-ca/learning/dns/what-is-a-dns-server/

Cloud Build - https://cloud.google.com/build/docs

Artifacts Registry - [https://cloud.google.com/artifact-registry](https://cloud.google.com/artifact-registry)

Static IP - https://cloud.google.com/compute/docs/ip-addresses/reserve-static-external-ip-address

## Prerequisites

- Google Cloud Subscription
- Basic knowledge of web client-server architecture
- Basic knowledge of Kubernetes
- Github account

## Environment Setup
 - Install gcloud - [https://cloud.google.com/sdk/docs/install](https://cloud.google.com/sdk/docs/install)

- Install kubectl - [https://kubernetes.io/docs/tasks/tools/](https://kubernetes.io/docs/tasks/tools/)

- Create a GCP Project using the GUI or gcloud command line

- Enable billing for the project -[https://console.cloud.google.com/billing/projects](https://console.cloud.google.com/billing/projects)

- enable container.googleapis.com service by running the following command -     `gcloud services enable container.googleapis.com`

## Setting up the Kubernetes Cluster

Follow the guidelines in this document to setup the GKE Cluster (DO NOT deploy the application yet) - [https://cloud.google.com/kubernetes-engine/docs/deploy-app-cluster](https://cloud.google.com/kubernetes-engine/docs/deploy-app-cluster)

## Deploying the application

- Clone this repo by running the following command
`git clone https://github.com/itsamk/gcp-application-pipeline.git`

- Reserving static-ip: A static-ip is needed to deploy the application securely with a SSL-certificate. Run the following command to provision a static-ip in the project.

        `gcloud compute addresses create ADDRESS_NAME --global`

- Point DNS to static-ip (OPTIONAL): In this step, add the following entry in your DNS record for your custom domain -

| Type | Name | Data | TTL |
|--|--|--|--|
| A | www | IP_ADDRESS | 600 seconds |

- Run the following command to the kubernetes 'deployment' for the application:
        `kubectl apply -f k8s/hello-app-depl.yaml`
- Run the following command to provision a managed certificate from Google:
        `kubectl apply -f k8s/managed-cert.yaml`
- Run the following command to deploy the ingress for the application:
        `kubectl apply -f k8s/ingress-srv.yaml`
    This will also deploy the Load Balancer component in Google that will be ingress for all your traffic.

## Setting up the build pipeline

### Step 1: Enable Artifact Registry API 
    Navigate to Artifact Registry in [Cloud Console](https://console.cloud.google.com) and Enable the Artifact Registry API. 
### Step 2: Create the Repository to store images
    Use the Cloud Consolte GUI to add a new repository with the name us.gcr.io (or gcr.io). Use defaults for rest of the fields and create the repository.
### Step 3: Create Github repo.
    Create a Github repository. (Or fork this one for the test.)
### Step 4: Enable Cloud Build API
    Navigate to Cloud Build in [Cloud Console](https://console.cloud.google.com) and Enable the Cloud Build API. 
### Step 5: Create Build Trigger
    Create build trigger with the following settings -


You are done! Now any new changes you push to your main branch on Github will trigger the build and deploy your application to the Kubernetes cluster in a secure way with no downtime.