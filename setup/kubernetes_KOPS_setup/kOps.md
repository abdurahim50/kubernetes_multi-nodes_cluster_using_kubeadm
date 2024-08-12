# Setup IAM User for Kubernetes

Follow this link to set up: [Kubernetes Operations (kops) on AWS](https://kops.sigs.k8s.io/getting_started/aws/)

## Install kops and kubectl

1. **Download and Install kops:**
    ```bash
    curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64
    chmod +x kops-linux-amd64
    sudo mv kops-linux-amd64 /usr/local/bin/kops
    ```

2. **Download and Install kubectl:**
    ```bash
    curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl
    chmod +x ./kubectl
    sudo mv ./kubectl /usr/bin/kubectl
    ```

## Setup AWS S3 Bucket

Create an S3 bucket for storing kops state:
```bash
aws s3 mb s3://scienteh.in.k8s --region us-east-1
```        
## Create Route53 Private Hosted Zone

Create a private hosted zone in AWS Route53. You can do this through the AWS Management Console or using the AWS CLI.

## Set up environment variables for kops

```
export KOPS_CLUSTER_NAME=jenkinstraining.com
export KOPS_STATE_STORE=s3://scienteh.in.k8s
```
Add these variables to your .bashrc file for persistent configuration:

## Edit .bashrc
```
vim /root/.bashrc
```
## Add the following lines to .bashrc
```
export KOPS_CLUSTER_NAME=jenkinstraining.com
export KOPS_STATE_STORE=s3://scienteh.in.k8s
```
## Apply the changes
```
source /root/.bashrc
```
## Generate an SSH key pair
```
ssh-keygen
```
## Create a Kubernetes cluster using kops
```
kops create cluster \
--state=${KOPS_STATE_STORE} \
--node-count=2 \
--master-size=t3.medium \
--node-size=t3.medium \
--zones=ap-south-1a,ap-south-1b \
--name=${KOPS_CLUSTER_NAME} \
--dns private \
--master-count 1
```
## View the cluster details
kops get cluster

## Update the cluster with the latest configuration
kops update cluster --name=${KOPS_CLUSTER_NAME} --yes --admin

## Validate the cluster status, waiting up to 10 minutes
kops validate cluster --wait 10m

## SSH into the master node
ssh -i ~/.ssh/id_rsa ubuntu@api.jenkinstraining.com

## Delete the Kubernetes cluster
kops delete cluster --yes

