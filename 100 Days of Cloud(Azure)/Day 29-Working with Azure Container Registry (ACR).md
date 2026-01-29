The Nautilus DevOps team has been tasked with setting up a containerized application. They need to create a Azure Container Registry (ACR) to store their Docker images. Once the repository is created, they will build a Docker image from a Dockerfile located on the azure-client host and push this image to the ACR repository. This process is essential for maintaining and deploying containerized applications in a streamlined manner.

1) Create a ACR repository named datacenteracr29971 under East US.

2) Pricing plan must be Basic.

3) Dockerfile already exists under /root/pyapp directory on azure-client host.

4) Build a Docker image using this Dockerfile and push the same to the newly created ACR repo. The image tag must be latest i.e datacenteracr29971:latest.
```bash
# Step 1: Create Azure Container Registry (ACR)
az acr create --resource-group <YourResourceGroup> --name datacenteracr29971 --sku Basic --location eastus
```

```bash
# Step 2: Log in to the ACR
az acr login --name datacenteracr29971
```
```bash
# Step 3: Build the Docker image
docker build -t datacenteracr29971.azurecr.io/datacenteracr29971:latest /root/pyapp
```
```bash
# Step 4: Push the Docker image to ACR
docker push datacenteracr29971.azurecr.io/datacenteracr29971:latest
```
```bash
# Step 5: Verify the image is pushed
az acr repository list --name datacenteracr29971 --output table
az acr repository show-tags --name datacenteracr29971 --repository datacenteracr29971 --output table
```
```bash
# Note: Replace <YourResourceGroup> with the actual resource group name where you want to create the ACR.
```
```bash
# Additional Note: Ensure that Docker is installed and running on the azure-client host before executing the build and push commands.
```
```bash
# Additional Note: Make sure you have the necessary permissions to create resources in the specified Azure subscription.
```
```bash
# Additional Note: You may need to configure your Docker client to authenticate with Azure Container Registry if you encounter any authentication issues.
```
```bash
# Additional Note: If you face any issues with the ACR login, you can also use the Azure CLI to get the login server and use Docker login command.
```
```bash
# Example Docker login command if needed
docker login datacenteracr29971.azurecr.io
```
```bash
# End of the steps for creating ACR, building, and pushing the Docker image.
```
```bash
# Remember to replace placeholders with actual values as needed.
```
