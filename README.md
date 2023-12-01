# How to Run Azure Data API Builder (DAB) in various Hosting Environments
[Azure Data API Builder](https://learn.microsoft.com/en-us/azure/data-api-builder/overview-to-data-api-builder?tabs=azure-sql) provides an ability to auto-generate APIs to expose data in database like MS SQL.

This repo provides step-by-step instructions to run Azure Data API Builder in various environments like:
* Local windows machine
* Docker Desktop
* Azure Container Instance
* Azure Kubernetes Service

## Run DAB in a Local Windows Machine
### Install DAB
1. Data API Builder requires .NET 6. Install .NET 6
    - To install .NET 6 download MSI from: [DotNet](https://dotnet.microsoft.com/en-us/download/dotnet) and execute the MSI
2. Install DAB
    `dotnet tool install --global Microsoft.DataApiBuilder`
3. Update DAB
	`dotnet tool update --global Microsoft.DataApiBuilder`
4. Verify DAB installation
    `dab --version`

### Install SQL Database
* [Instructions for installing Azure SQL Database](https://github.com/git-vp/azure-data-api-builder/blob/main/install-sql-db.md)

### Generate DAB Configuration File
* [Instructions for generating DAB config file](https://github.com/git-vp/azure-data-api-builder/blob/main/generate-dab-config.md)

### Run DAB locally
1. Now start the Data API builder:
    `dab start --verbose --no-https-redirect`
    `Note:` without --no-https-redirect, DAB will throw 307 (Redirect) error







