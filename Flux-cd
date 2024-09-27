Flux CD
=============================================
Prerequisites
AKS Cluster: Make sure you have a running AKS cluster and have access to it via kubectl.
Azure CLI: Install and configure the Azure CLI (az).
kubectl: Make sure you have the kubectl tool installed and configured to communicate with your AKS cluster.
Flux CLI: Install the Flux CLI.
GitHub or GitLab Repository: You need a Git repository to store the configurations.
=============================================


# create vm and install the following

# open power shell and connect to the vm

ssh -i "C:\Users\MartujNadaf\Downloads\demo_key.pem" azureuser@20.118.211.217

sudo su -
 
sudo apt-get update

# isntall flux cli

curl -s https://fluxcd.io/install.sh | sudo bash
flux --version

# git should be installed by default. to check 

git

# install az cli 

curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash

# install kubelet
vi  kubectl.sh

curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
kubectl version

save the file and 

chmod +x kubectl.sh
./kubectl.sh


# connect to the cluster
az account set --subscription 607e05a3-6e68-4027-bef6-4184bd3922b9
az aks get-credentials --resource-group Martuj-aks-test --name martuj-aks-cluster --overwrite-existing

kubectl get no


# create hithub token and export it

export GITHUB_TOKEN=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A

env | grep  GIT

# pre check


flux check --pre


# Now install controller on GitHub and in the cluster

flux bootstrap github \
  --token-auth \
  --owner=martuj \
  --repository=flux-demo2 \
  --branch=main \
  --path= clusters/dev \
  --personal


-----------------------------------
Explain:
syntax:

flux bootstrap github \
  ----token-auth \
  --owner=<github-username> \
  --repository=<repository-name> \
  --branch=main \
  --path=clusters/aks \
  --personal



Explanation:
--token-auth : get from the export 
--owner: Your GitHub username.
--repository: The name of your GitHub repository where Flux will store cluster configurations.
--branch: Branch of your repository (e.g., main or master).
--path: Directory within your repository to store Flux configurations (e.g., clusters/aks).
--personal: Use this flag for personal GitHub repositories.
This command does the following:

Installs the Flux controllers (source-controller, kustomize-controller, helm-controller, and notification-controller).
Creates a GitRepository and Kustomization resource in the flux-system namespace to point to your GitHub repository.

----------------------


#  Verify the Flux Installation

kubectl get pods -n flux-system

# Check the Flux GitRepository and Kustomization resources:

flux get sources git
flux get kustomizations


# configure git 

git config --global user.email "< Add your email >"
git config --global user.name "< Add your Name >"

# other some useful command
git status
git add 
git log
git remote add origin https://github.com/martuj/flux-cd.git
git remote show origin
git push origin master

# clone the repo 

git clone https://github.com/martuj/flux-demo2.git

# go inside ht flux-demo1 dir 

create dir apps and inside the create yml files

vi nginx-deployment.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
  namespace: default
  labels:
    app: nginx
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.21
        ports:
        - containerPort: 80


vi nginx-service.yaml

apiVersion: v1
kind: Service
metadata:
  name: nginx
  namespace: default
spec:
  type: LoadBalancer
  selector:
    app: nginx
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80
