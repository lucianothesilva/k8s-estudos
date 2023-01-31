# kubernetesStuff

## Modo imperativo de imagem no ACR

Detailed how to deploy this curriculum in a container in kubernetes in azure

Here's a detailed guide on how to deploy the HTML website curriculum in a container in a Kubernetes cluster on Azure:

1. Package the HTML code into a Docker container (with index.html name):
    - Create a **`Dockerfile`** with the following content:

```
# Use the Nginx image as the base image
FROM nginx

# Copy the HTML code into the container
COPY . /usr/share/nginx/html

# Set the default command to start Nginx
CMD ["nginx", "-g", "daemon off;"]

```

- Build the Docker image using the following command:

```
docker build -t my-curriculum:1.0 .
```

- Push the Docker image to a container registry, such as Azure Container Registry (ACR) or Docker Hub. If you use ACR, you can use the following commands to log in, tag the image, and push it:

```
# Log in to ACR
az acr login --name curriculumcr

# Tag the image with the ACR repository
docker tag my-curriculum:1.0 curriculumcr.azurecr.io/my-curriculum:1.0

# Push the image to ACR
docker push curriculumcr.azurecr.io/my-curriculum:1.0
```

1. Create a Kubernetes cluster on Azure:
    - Use the Azure CLI to create an AKS (Azure Kubernetes Service) cluster:

```
az account set --subscription Testes

RESOURCEGROUP=curriculum-group
az group create -n $RESOURCEGROUP -l brazilsouth

az aks create \
  --name curriculum-cluster \
  --resource-group curriculumRG \
  --node-count 1 \
  --generate-ssh-keys \
	--attach-acr curriculumcr
```

- Connect to the Kubernetes cluster using the following command:

```
az aks get-credentials --name curriculum-cluster --resource-group curriculumRG
```

1. Deploy the container to the Kubernetes cluster:
    - Use the following **`kubectl`** command to create a Kubernetes deployment:

```bash
kubectl create deployment my-curriculum --image=curriculumcr.azurecr.io/my-curriculum:1.0
```

- Expose the deployment as a Kubernetes service using the following **`kubectl`** command:

```
kubectl expose deployment my-curriculum --type=LoadBalancer --port=80
```

1. Access the website:
    - Use the following **`kubectl`** command to retrieve the public IP address of the service:

```
kubectl get service my-curriculum
```

- Access the website using a browser and the IP address from the previous step.
