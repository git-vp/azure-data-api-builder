# Steps for Running Azure Data API Builder (DAB)
[Azure Data API Builder](https://learn.microsoft.com/en-us/azure/data-api-builder/overview-to-data-api-builder?tabs=azure-sql) provides an ability to auto-generate APIs to expose data in database like MS SQL.

This repo provides step-by-step instructions to run Azure Data API Builder in various environments, like running on a VM, Azure Container Instance, Docker etc

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

## Test APIs
### Test APIs in Browser
1. Open a browser and enter `http://localhost:5000/api/Book`. This should show all the books.
2. Enter `http://localhost:5000/api/Author` in the browser. This should show all the authors.

### Test APIs in POSTMAN
1. To retrieve all books in POSTMAN using REST API:
     Method: `GET`
     URL: `http://localhost:5000/api/Book`
2. To retrieve all authors in POSTMAN using REST API:
     Method: `GET`
     URL: `http://localhost:5000/api/Author`
3. To retrieve first 5 books using GraphQL:
     URL: `http://localhost:5000/api/Author`
     Query:
     ```
      {
        books(first: 5, orderBy: { title: DESC }) {
          items {
            id
            title
          }
        }
      }
     ```






