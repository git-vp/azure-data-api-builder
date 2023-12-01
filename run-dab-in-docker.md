# Run DAB in Docker
See below for instructions to run DAB in Docker

## Pre-requisites
* Ensure you are able to run `docker` command on your terminal

## Install SQL Database
* [Instructions for Installing Azure SQL Database](https://github.com/git-vp/azure-data-api-builder/blob/main/install-sql-db.md)

## Generate DAB Configuration File
* [Instructions for Generating DAB Config File](https://github.com/git-vp/azure-data-api-builder/blob/main/generate-dab-config.md)

## Run DAB in Docker
* To run DAB in Docker:
  `docker run -it -v "C:\Users\Venu\Samples\DataApiBuilder:/App/configs" -p 5000:5000 mcr.microsoft.com/azure-databases/data-api-builder:latest --ConfigFileName ./configs/dab-config-books-simulator-auth.json`
* In the above command:
  * Local directory `C:\Users\Venu\Samples\DataApiBuilder` is mapped to `/App/configs` directory in the container
  * Host machine (where Docker is running) port 5000 is mapped to container port 5000, where the data-api-builder is running and listening on port 5000
  * Since local directory `C:\Users\Venu\Samples\DataApiBuilder` is mapped to `/App/configs`, the configuration file `dab-config-books-simulator-auth.json` is available in `/App/configs` directory in the container
 
