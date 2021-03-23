# EKS with Terraform > Deploy Argocd > Multi tier application

This Repository will guide you to install EKS cluster by terraform. Then Deploy ArgoCD in EKS cluster. With ArgoCD will build an CD pipeline to deploy multi tier application from github repository. 

### Requirements
- gitcli and github
- aws account
- terraform
- awscli
- aws-iam-authenticator
- kubectl
- argocd and argocd cli

### 1. AWS Account
We need Access Key ID and Secret Access Key to login AWS cli from my VS Code

### 2. Terraform CLI
To Download and install Terraform cli please visit https://www.terraform.io/downloads.html

### 3. AWSCLI 2
To Download and install AWS CLI please visit https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html

### 4. AWS-IAM-AUTHENTICATOR
After EKS created we need to have aws-iam-authenticator to access it. To install it please visit https://docs.aws.amazon.com/eks/latest/userguide/install-aws-iam-authenticator.html

### 5. Kubectl
To configure EKS we need kubectl. To download and install please visit https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html

### 6. ArgoCD CLI 
- ``` wget https://github.com/argoproj/argo-cd/releases/download/v1.7.14/argocd-linux-amd64 -O argocd ``` - To download ArgoCD cli
- ``` chmod +x argocd ``` - To make the folder executable 
- ``` sudo mv argocd /usr/local/bin``` -To use ArgoCD cli in your terminal

### 7. Install EKS with Terraform 
- Login to AWS by using aws cli 
- ``` git clone https://github.com/hashicorp/terraform-provider-aws.git ```
- ``` cd terraform-provider-aws/examples/eks-getting-started ```
- ``` terraform init ```
- Change accourding to your need in .tf files
- ``` terraform plan ```
- terraform apply and yes for confirmation
- ``` mkdir ~/.kube && touch ~/.kube/config ```
- ``` terraform output kubeconfig > ~/.kube/config ```(Need to edit "~/.kube/config" to get rid of first and last line)
- ``` kubectl cluster-info ```

For more information visit https://learn.hashicorp.com/tutorials/terraform/eks

### 8. Install ArgoCD in EKS cluster

- ``` kubectl create namespace argocd ```- To create a seperate namespace 
- ``` kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml ```- To install 
- ``` kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}' ```- To expose service to outside
- ``` kubectl get svc argocd-server -n argocd | awk '{print $4}' ```- To get the web url for ArgoCD login page
- ``` kubectl get pods -n argocd -l app.kubernetes.io/name=argocd-server -o name | cut -d'/' -f 2 ```- To get name of the ArgoCD pod
- ``` argocd login "WB_URL_THAT_GOT_EARLIER" --insecure ``` - To login ArgoCD with cli
- To login to ArgoCD UI user username as "admin" and password is the name of the pod we got earlier

For more information please visit https://argoproj.github.io/argo-cd/getting_started/

### 9. Create CD with ArgoCD 
Here I am trying to host my application in github. So anything i will change my git Repository will change deployment in EKS
- Log in to argoCD with cli
- I have created a public repo in https://github.com/bappy776/3-tier-application and yaml folder got all my deployment. I would recommend to use private repo for security purpose.
- ``` argocd app create argocd-wordpress --repo https://github.com/bappy776/3-tier-application --path yaml --dest-namespace default --dest-server https://kubernetes.default.svc --project default --sync-policy auto --self-heal ``` - To create an application in ArgoCD, connect the application with github repo for CD. Anything change in Github repo will take effect in EKS cluster deployment.




