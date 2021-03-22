# EKS with Terraform > Deploy Argocd > Multi tier application

This Repository will guide you to install EKS cluster by terraform. Then Deploy ArgoCD in EKS cluster. With ArgoCD will build an CD pipeline to deploy multi tier application from github repository. 

### Requirements 
- aws account
- terraform
- awscli
- aws-iam-authenticator
- kubectl
- argocd

### 1. AWS Account
We need Access Key ID and Secret Access Key to login AWS cli from my VS Code

### 2. Terraform CLI
To Download and install Terraform cli please visit https://www.terraform.io/downloads.html

### 3. AWSCLI 2
To Download and install AWS CLI please visit https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html

### 4. AWS-IAM-AUTHENTICATOR
After EKS created we need to have aws-iam-authenticator to access it. To install it please visit https://docs.aws.amazon.com/eks/latest/userguide/install-aws-iam-authenticator.html

### 5. Kubectl
To configure we need kubectl. To download and install please visit https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html

### 6. Install EKS with Terraform 
- Login to AWS by using aws cli 
- git clone https://github.com/hashicorp/terraform-provider-aws.git
- cd terraform-provider-aws/examples/eks-getting-started
- terraform init
- Change accourding to your need in .tf files
- terraform plan 
- terraform apply and yes for confirmation
- terraform output kubeconfig > ~ /.kube/config (Need to edit "~/.kube/config" to get rid of first and last line)
- kubectl cluster-info

For more information visit https://learn.hashicorp.com/tutorials/terraform/eks

### 7. Install ArgoCD in EKS cluster

- kubectl create namespace argocd - To create a seperate namespace 
- kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml - To install 
- kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}' - To expose service to outside
- kubectl get svc argocd-server -n argocd | awk '{print $4}' - To get the url for ArgoCD login page
- kubectl get pods -n argocd -l app.kubernetes.io/name=argocd-server -o name | cut -d'/' -f 2 - To get name of the ArgoCD pod
- To login to ArgoCD UI user username as "admin" and password is the name of the pod

For more information please visit https://argoproj.github.io/argo-cd/getting_started/

### 8. Create CD with ArgoCD 
Here I am trying to host my application in github. So anything i will change my git Repository will change deployment in EKS
- Setting > Repository > Connect repo using HTTPS
- Add https://github.com/bappy776/3-tier-application as Repository URL and connect
- Application > NEW APP 
- ![image](https://user-images.githubusercontent.com/10797214/111900849-ebbab800-8a88-11eb-8606-941815651442.png)
- Keep https://kubernetes.default.svc if ArgoCD running same cluster and default for default namespace
- kubectl get svc wordpress | awk '{print$4}' - To get the url for my wordpress that i have deploy in default namespace




