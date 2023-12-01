# Run DAB in Azure Containter Instance
See below for instructions to run DAB in Azure Container Instance (ACI)

## Pre-requisites
* Ensure you are able to run `docker` command on your terminal

## Install SQL Database
* [Instructions for Installing Azure SQL Database](https://github.com/git-vp/azure-data-api-builder/blob/main/install-sql-db.md)

## Generate DAB Configuration File
* [Instructions for Generating DAB Config File](https://github.com/git-vp/azure-data-api-builder/blob/main/generate-dab-config.md)

## Run DAB in Azure Container Instance (ACI)
1. Create a Storage Account
	 `az storage account create --name dabdemovp28112023 --resource-group dab-demo-rg --sku Standard_LRS --location westeurope` 
	
2. Create a File Share in the Storage Account
   `az storage share create --name books-dab-config --account-dabdemovp28112023 `
  
3. Upload dab-config.json from the above [Generate DAB Configuration File](https://github.com/git-vp/azure-data-api-builder/blob/main/generate-dab-config.md)) to the file share

4. Get account key for Azure Files Storage Account:
   `az storage account keys list -g dab-demo -n dabdemovp28112023 --query [0].value -o tsv`
  
5. Run ACI (Azure Container Instance:
   `az container create -g dab-demo-rg --name dab-aci --dns-name-label books-demo-api --image mcr.microsoft.com/azure-databases/data-api-builder:latest --ports 5000 --ip-address public --cpu 2 --memory 4 --os-type Linux --azure-file-volume-mount-path /books-dab-config --azure-file-volume-account-name dabdemovp28112023 --azure-file-volume-account-key <account-key> --azure-file-volume-share-name books-dab-config --command-line "sh -c 'cp -f /books-dab-config/dab-config.json /App/ ; dotnet Azure.DataApiBuilder.Service.dll'" -o json`

