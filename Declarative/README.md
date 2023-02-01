Detailed commands how to deploy this curriculum in a container in kubernetes in azure

Here are the detailed commands to deploy a containerized HTML curriculum in Kubernetes on Azure:

1. Create an Azure account or log in to an existing one:

```
az login
```

2. Create a Kubernetes cluster in Azure using Azure Kubernetes Service (AKS):

```
az aks create --name curriculumCluster --resource-group curriculumRG --node-count 1 --generate-ssh-keys
```

3. Containerize the HTML curriculum by creating a Docker image. You can use a **`Dockerfile`** to automate the process. Here's an example Dockerfile:

```
FROM nginx:alpine
COPY index.html /usr/share/nginx/html
```

4. Build the Docker image:

```
docker build -t mycurriculum:v1 .
```

5. Push the Docker image to a container registry such as Docker Hub or Azure Container Registry (ACR). To push to Docker Hub, you need to first log in:

```
docker login
docker push mycurriculum:v1
```

6. Connect to the AKS cluster and set the context for **`kubectl`**:

```
az aks get-credentials --name curriculumCluster --resource-group curriculumRG
```

7. Create a Kubernetes deployment.yaml and service in your AKS cluster using YAML manifests. 

8. Apply the deployment and service manifests:

```
kubectl apply -f deployment.yaml
```

9. Use the following command to get the service's public IP to verify that the curriculum website is accessible through the public IP or hostname assigned by the Load Balancer. 

```
kubectl get service mycurriculum
```
