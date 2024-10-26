# AKS VNET Preview Testing

A repo for testing the AKS VNET preview feature.

```bash

# Setup some variables to use
TENANT_ID=16b3c013-d300-468d-ac64-7eda0820b6d3
LOCATION=centralus
BASE_NAME=cdw-aksvnettesting-20241015

# Log into Azure
az login -t $TENANT_ID

# Create the resource group
az group create -l $LOCATION -n $BASE_NAME

# Create the cluster
az aks create \
-g $BASE_NAME \
-n $BASE_NAME-aks \
--generate-ssh-keys \
--enable-managed-identity \
--node-count 1 \
--network-plugin azure \
--enable-addons monitoring \
--enable-apiserver-vnet-integration \
--enable-file-driver


```

