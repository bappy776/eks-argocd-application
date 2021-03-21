# EKS with Terraform > Deploy Argocd > Multi tier application

This Repository will guide you to install EKS cluster by terraform. Then Deploy ArgoCD in EKS cluster. With ArgoCD will build an CD pipeline to deploy multi tier application from github repository

### Requirements 
- aws account
- terraform
- awscli
- aws-iam-authenticator
- kubectl
- argocd

### AWS Account
We need Access Key ID and Secret Access Key to login AWS cli

### Terraform CLI
To Download and install Terraform cli please visit https://www.terraform.io/downloads.html

### AWSCLI 2
To Download and install AWS CLI please visit https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html

### AWS-IAM-AUTHENTICATOR
After EKS created we need to have aws-iam-authenticator to access it. To install it please visit https://docs.aws.amazon.com/eks/latest/userguide/install-aws-iam-authenticator.html

### Kubectl
To configure we need kubectl. To download and install please visit https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html

### Install EKS with Terraform 
- Login to AWS by using aws cli 
- git clone https://github.com/hashicorp/terraform-provider-aws.git
- cd /terraform-provider-aws/examples/eks-getting-started
- terraform init
- Change accourding to your need in .tf files
- terraform plan 
