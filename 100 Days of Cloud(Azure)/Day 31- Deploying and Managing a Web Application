The Nautilus DevOps team is tasked with deploying a Python-based web application on Azure. You need to create a web app using the following specifications:

1) The Web App name should be devops-webapp.
2) It should be created in the West US region under the default resource group.
3) The publish option should be set to Code.
4) The Runtime Stack should be Python with Linux as the operating system.
5) Create a new App Service Plan named devops-learn-python with the SKU Basic B1.
6) Application Insights should be disabled.
7) Add tags:

Name: WebAppLearning
Environment: Dev
Make sure the web app is in Running state after creation.
```bash
# Step 1: Create a resource group (if not already created)
az group create --name default --location westus
```
```bash
# Step 2: Create an App Service Plan
az appservice plan create --name devops-learn-python --resource-group default --location westus --sku B1 --is-linux
```
```bash
# Step 3: Create the Web App
az webapp create --name devops-webapp --resource-group default --plan devops-learn-python --runtime "PYTHON|3.8" --deployment-local-git
```
```bash
# Step 4: Disable Application Insights
az webapp config appsettings set --name devops-webapp --resource-group default --settings "APPINSIGHTS_INSTRUMENTATIONKEY="
```
```bash
# Step 5: Add tags to the Web App
az webapp update --name devops-webapp --resource-group default --set tags.Name=WebAppLearning tags.Environment=Dev
```
```bash
# Step 6: Verify the Web App is in Running state
az webapp show --name devops-webapp --resource-group default --query "state"
```
```bash
# Note: Ensure that you have the necessary permissions to create resources in the specified Azure subscription.
```
```bash
# Additional Note: You may need to configure firewall rules to allow access to the Web App from your IP address.
```
```bash
# Additional Note: Make sure to monitor the deployment status to confirm that the Web App reaches the Running state.
```
```bash
# End of the steps for creating the Web App.
```
```bash