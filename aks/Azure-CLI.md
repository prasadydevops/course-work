# Azure CLI Introduction

## Create a resource group

```bash
az group create --name myResourceGroup --location westus
```

## Create a virtual machine

```bash
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --generate-ssh-keys
```

## Connect to the virtual machine

```bash
ssh azureuser@<public-ip-address> # From cloud shell user name will be $USER
```

## List resource groups

```bash
az group list
```

## Delete a resource group

```bash
az group delete --name myResourceGroup
```

Ref: https://learn.microsoft.com/en-us/azure/cloud-shell/quickstart?source=recommendations

## Scripting with Azure CLI