# HarvestHelper.Infra

Infrastructure components used in HarvestHelper

## Add the GitHub package source
```powershell
$owner="HarvestHelper"
$gh_pat="[PAT HERE]"

dotnet nuget add source --username Duarte --password $gh_pat --store-password-in-clear-text --name github "https://nuget.pkg.github.com/$owner/index.json"
```
