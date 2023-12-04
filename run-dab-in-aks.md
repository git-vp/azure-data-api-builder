# Run DAB in Azure Kubernetes Service (AKS)
See below for instructions to run DAB in Azure Kubernetes Service (AKS). These instructions can be followed to run DAB either in Simulator auth mode or AAD auth mode.

## Create AKS Cluster
1. Create a resource group

   `az group create --location westus --resource-group whatthehack-rg`
   
2. Create 1-node AKS cluster

   `az aks create --location westeurope --name wth-aks-cluster --node-count 1 --no-ssh-key --resource-group whatthehack-rg --zones 1 --enable-managed-identity`
   
3. Get AKS cluster credentials

   `az aks get-credentials --resource-group whatthehack-rg --name wth-aks-cluster`

## Install SQL Database
* [Instructions for Installing Azure SQL Database](https://github.com/git-vp/azure-data-api-builder/blob/main/install-sql-db.md)

## Generate DAB Configuration File
* [Instructions for Generating DAB Config File](https://github.com/git-vp/azure-data-api-builder/blob/main/generate-dab-config.md)

## Run DAB in Azure Container Instance (ACI)
1. To run DAB in AKS, DAB requires dab-config.json. This json file can be passed to AKS in a few ways, one of them using `Config Map`
   
2. To pass dab-config.json as a Config Map, create a config map k8s object

   * To use AAD auth mode, use `dab-config-books-aad-auth.json`
   `kubectl create configmap dab-config --from-file=dab-config-books-aad-auth.json`

   * To use Simulator auth mode, use `dab-config-books-simulator-auth.json`
   `kubectl create configmap dab-config --from-file=dab-config-books-simulator-auth.json`

4. Check that the config map k8s object is correctly created

   `kubectl describe cm dab-config`
   
5. Create [dab-k8s-deployment-service.yaml](https://github.com/git-vp/azure-data-api-builder/edit/main/dab-k8s-deployment-service.yaml) manifest.
   Note: this manifest uses [dab-config-config-sumulator-auth.json](https://github.com/git-vp/azure-data-api-builder/edit/main/dab-config-config-sumulator-auth.json). Repalce `dab-config-config-sumulator-auth.json` with `dab-config-config-aad-auth.json` should you want to use AAD auth.

7. In the above deployment manifest, dab-config-books-simulator-auth.json file is made available to the pod at /App/config directory
8. The service listens on port 80, forwards the traffic to port 5000 on the pod, and the container in the pod listens on port 5000
9. Create the k8s deployment and service objects

   `kubectl apply -f dab-k8s-deployment-service.yaml`

10. Check that the k8s deployment and service objects are created correctly

   `kubectl get services`
   
   `kubectl get deployments`
   
