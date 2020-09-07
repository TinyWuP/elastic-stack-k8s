# aks terraform provisioner

Provisions a Managed Kubernetes Cluster in AKS via Terraform

## Requirements
- Terraform v0.13.1
- hashicorp/azurerm version 2.26.0

## Setup (requires az CLI)

``` bash
#prompt login
az login

# get accounts (get your subscriptionid)
az account list

# set account to use
az account set --subscription <subscriptionId>

# Show account details (get subscriptionId and tenantId)
az account show --query "{subscriptionId:id, tenantId:tenantId}"

# Create Role Contributor (appId==client_id, password==your_client_secret)
az ad sp create-for-rbac --name elastic-aks --role="Contributor" --scopes="/subscriptions/<YOUR_SUBSCRIPTION_ID>"

# init
terraform init

# plan
terraform plan -var "client_id=appId" -var "client_secret=secret" -out=elastic-aks.tfplan

# apply (this will create the cluster!!)
terraform apply elastic-aks.tfplan
```

# Setup Kubernetes dashboard

``` bash
az aks get-credentials --resource-group elastic-aks --name elastic-aks --overwrite-existing

# rbac users
kubectl create clusterrolebinding kubernetes-dashboard --clusterrole=cluster-admin  --serviceaccount=kube-system:kubernetes-dashboard

# open dashboard
az aks browse --resource-group elastic-aks --name elastic-aks
```