# How to Run Azure Data API Builder (DAB) in various Hosting Environments
[Azure Data API Builder](https://learn.microsoft.com/en-us/azure/data-api-builder/overview-to-data-api-builder?tabs=azure-sql) provides a capability to auto-generate REST and GraphQL API endpoints based on a database schema without writing an API application.

Azure Data API Builder is open source and can be hosted on any platform.

## Authentication and Authorization
Clients using API provided by Data API Builder can use the following authentication methods to authenticate the client with Data API builder:
* Azure Static Web Apps
* Azure AD - provides a JWT token which the client will supply in the AUTHORIZATION Bearer token HTTP header to identify the client.
  
In this article, I am going to focus on Azure AD.

Once the client calling the API is authenticated, actions performed by the client is authorised by specifying the role assigned to the client in X-MS-API-ROLE HTTP header.

In summary, provide the following values in HTTP header:
* To authenticate - specify JWT bearer token in AUTHORIZATION Bearer token
* To authorize - specify role in X-MS-API-ROLE

![Data API Builder](https://github.com/git-vp/azure-data-api-builder/assets/25417872/142ce6ee-ba93-4f21-ad82-a1b9e6137315)


## Hosting Data API Builder
This repo provides step-by-step instructions to run Azure Data API Builder in various environments like:
* Local windows machine
* Docker Desktop
* Azure Container Instance
* Azure Kubernetes Service

## Run DAB in a Local Windows Machine
* [Instructions for Running DAB in a Local Windows Machine](https://github.com/git-vp/azure-data-api-builder/blob/main/run-dab-in-localwindowsmachine.md)

## Run DAB in Docker 
* [Instructions for Running DAB in Docker](https://github.com/git-vp/azure-data-api-builder/blob/main/run-dab-in-docker.md)

## Run DAB in Azure Container Instance
* [Instructions for Running DAB in ACI](https://github.com/git-vp/azure-data-api-builder/blob/main/run-dab-in-aci.md)

## Run DAB in Azure Kubernetes Service
* [Instructions for Running DAB in AKS](https://github.com/git-vp/azure-data-api-builder/blob/main/run-dab-in-aks.md)

## Test DAB
* [Instructions for Testing DAB DB Endpoints](https://github.com/git-vp/azure-data-api-builder/blob/main/test-dab.md)






