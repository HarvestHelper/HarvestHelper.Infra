# HarvestHelper.Infra

Infrastructure components used in HarvestHelper

## Add the GitHub package source
```powershell
$owner="HarvestHelper"
$gh_pat="[PAT HERE]"

dotnet nuget add source --username Duarte --password $gh_pat --store-password-in-clear-text --name github "https://nuget.pkg.github.com/$owner/index.json"
```
## Azure
### Creating an Azure resource group
```powershell
$appname="harvesthelper"
az group create --name $appname --location westeurope
```

### Creating an Azure Cosmos DB account
```powershell
$appname="harvesthelper"
az cosmosdb create --name $appname --resource-group $appname --kind MongoDB --enable-free-tier
```

### Creating an Azure service bus namespace
```powershell
$appname="harvesthelper"
az servicebus namespace create --name $appname --resource-group $appname --sku Standard
```

### Creating a Container Registry
```powershell
$appname="harvesthelper"
az acr create --name $appname --resource-group $appname --sku Basic
```

### Creating a AKS cluster
```powershell
$appname="harvesthelper"

az extension add --name aks-preview
az feature register --name "EnableWorkloadIdentityPreview" --namespace "Microsoft.ContainerService"

az feature list -o table

az provider register --namespace Microsoft.ContainerService

az aks create -n $appname -g $appname --node-vm-size Standard_B2s --node-count 2 --attach-acr $appname --enable-oidc-issuer --enable-workload-identity

az aks get-credentials --name $appname --resource-group $appname
```

### Creating a Azure Ky vault
```powershell
$appname="harvesthelper"

az keyvault create -n $appname -g $appname

```

### Installing Emissary-Ingress 
```powershell
helm repo add datawire https://app.getambassador.io
helm repo update

kubectl apply -f https://app.getambassador.io/yaml/emissary/3.6.0/emissary-crds.yaml
kubectl wait --timeout=90s --for=condition=available deployment emissary-apiext -n emissary-system

$namespace="emissary"
$appname="harvesthelper"
helm install emissary-ingress datawire/emissary-ingress --set service.annotations."service\.beta\.kubernetes\.io/azure-dns-label-name"=$appname -n $namespace --create-namespace
kubectl rollout status deployment/emissary-ingress -n $namespace -w
```

### Config Emissary-Ingress routing
```powershell
$namespace="emissary"
kubectl apply -f .\emissary-ingress\listener.yaml -n $namespace
kubectl apply -f .\emissary-ingress\mappings.yaml -n $namespace
```