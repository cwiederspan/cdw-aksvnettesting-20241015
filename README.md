# AKS VNET Preview Testing

A repo for testing the AKS VNET preview feature.

```bash

# Setup some variables to use
TENANT_ID=16b3c013-d300-468d-ac64-7eda0820b6d3
LOCATION=centralus
BASE_NAME=cdw-aksvnettesting-20241015
STORAGE_NAME=cdwaks20241015

# Log into Azure
az login -t $TENANT_ID

# Enable the feature here
az feature register --namespace "Microsoft.ContainerService" --name "EnableAPIServerVnetIntegrationPreview"

# Check the status of the feature
az feature show --namespace "Microsoft.ContainerService" --name "EnableAPIServerVnetIntegrationPreview"

# Create the resource group
az group create -l $LOCATION -n $BASE_NAME

# Create the storage account
az storage account create \
-g $BASE_NAME \
-n $STORAGE_NAME \
-l $LOCATION \
--sku Standard_LRS

# Delete the storage account if needed
az storage account delete -g $BASE_NAME -n $STORAGE_NAME

# Create the cluster
az aks create \
-g $BASE_NAME \
-n $BASE_NAME-aks \
-l $LOCATION \
--generate-ssh-keys \
--enable-managed-identity \
--node-count 1 \
--network-plugin azure \
--enable-addons monitoring \
--enable-apiserver-vnet-integration

az aks get-credentials \
-g $BASE_NAME \
-n $BASE_NAME-aks

alias k=kubectl

k apply -f setup-public.yaml

k delete -f setup-public.yaml


# Use the bastion to connect and troubleshoot
k apply -f bastion.yaml

k exec -it bastion -- sh

```