# Deploying Images in Kubernetes from a Private Docker Repository

This guide explains how to deploy container images from a private Docker repository into a Kubernetes cluster.

Prerequisites
• A Kubernetes cluster (Minikube, GKE, EKS, AKS, etc.)
• kubectl CLI installed and configured
• Docker installed and authenticated to the private registry
• Access to a private Docker repository (Docker Hub, AWS ECR, GCR, or any private registry)

## Step 1: Login to AWS from your command line and create a Kubernetes Secret for Docker Registry Authentication

Kubernetes requires a secret to pull images from private registries. Run the following command:

kubectl create secret docker-registry <secret-name> \
--docker-server=*registry-url* \
--docker-username=*username* \
--docker-password=*password* \
--docker-email=*email*

### If your image is found on Docker Hub:

kubectl create secret docker-registry my-registry-key \
--docker-server=https://index.docker.io/v1/ \
--docker-username=myusername \
--docker-password=mypassword \
--docker-email=myemail@example.com

### For AWS ECR images, use:

kubectl create secret docker-registry my-registry-key \
--docker-server=https://**aws-account-id**.dkr.ecr.**region**.amazonaws.com \
--docker-username=AWS \
--docker-password=**password**

**password** can be generated using :
 
    ***aws ecr get-login-password*** 
which is gotten from your AWS ECR push commands

## Step 2: Deploy to Kubernetes

Apply the deployment:

***kubectl apply -f deployment.yaml***

### Verify the deployment:

***kubectl get pods***

If the pod fails to pull the image, check logs:

kubectl describe pod ***pod-name***
or kubectl logs ***pod-name***