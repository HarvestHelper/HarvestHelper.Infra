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