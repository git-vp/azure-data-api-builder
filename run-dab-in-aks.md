# Run DAB in Azure Kubernetes Service (AKS)
See below for instructions to run DAB in Azure Kubernetes Service (AKS)

## Pre-requisites
1. Create 1-node AKS cluster
2. Get AKS cluster credentials

## Install SQL Database
* [Instructions for Installing Azure SQL Database](https://github.com/git-vp/azure-data-api-builder/blob/main/install-sql-db.md)

## Generate DAB Configuration File
* [Instructions for Generating DAB Config File](https://github.com/git-vp/azure-data-api-builder/blob/main/generate-dab-config.md)

## Run DAB in Azure Container Instance (ACI)
1. To run DAB in AKS, DAB requires dab-config.json. This json file can be passed to AKS in a few ways, one of them using `Config Map`
   
2. To pass dab-config.json as a Config Map, create a config map k8s object

   `kubectl create configmap dab-config --from-file=dab-config-books-simulator-auth.json`

3. Check that the config map k8s object is correctly created

   `kubectl describe cm dab-config`
   
4. Create `dab-k8s-deployment-service.yaml manifests`

   ```yaml

    apiVersion: apps/v1
		kind: Deployment
		metadata:
		  name: data-api-builder
		spec:
		  replicas: 1
		  selector:
		    matchLabels:
		      app: data-api-builder
		  template:
		    metadata:
		      labels:
		        app: data-api-builder
		    spec:
		      containers:
		      - name: data-api-builder
		        image: mcr.microsoft.com/azure-databases/data-api-builder:latest
		        ports:
		        - containerPort: 5000
		        command: ["/bin/sh", "-c"]
		        args: ["dotnet Azure.DataApiBuilder.Service.dll --ConfigFileName /App/configs/dab-config-books-simulator-auth.json"]
		        volumeMounts:
		        - name: config-volume
		          mountPath: /App/configs
		      volumes:
		      - name: config-volume
		        configMap:
		          name: dab-config
    ---
   
		apiVersion: v1
		kind: Service
		metadata:
		  name: data-api-builder
		spec:
		  selector:
		    app: data-api-builder
		  ports:
		    - protocol: TCP
		      port: 80
		      targetPort: 5000
		  type: LoadBalancer
   
   ```
6. In the above deployment manifest, dab-config-books-simulator-auth.json file is made available to the pod at /App/config directory
7. The service listens on port 80, forwards the traffic to port 5000 on the pod, and the container in the pod listens on port 5000
8. Create the k8s deployment and service objects

   `kubectl apply -f dab-k8s-deployment-service.yaml`

9. Check that the k8s deployment and service objects are created correctly

   `kubectl get services`
   
   `kubectl get deployments`
   
