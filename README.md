# Azure Python Function App - Quick Setup Guide

A simple Python Azure Function App with HTTP trigger using the v2 programming model.

## Prerequisites

- Azure CLI installed and logged in
- Azure Functions Core Tools v4
- Python 3.7+

## Quick Setup

1. Create Azure Resources
    ```
   # Create resource group
    az group create --name myResourceGroup --location "East US"
    
    # Create storage account (must allow public access for Consumption plan)
    az storage account create \
      --name mystorageaccount \
      --location "East US" \
      --resource-group myResourceGroup \
      --sku Standard_LRS
    
    # Create Function App (MUST use --os-type linux for Python)
    az functionapp create \
      --resource-group myResourceGroup \
      --consumption-plan-location "East US" \
      --runtime python \
      --runtime-version 3.12 \
      --functions-version 4 \
      --name myFunctionApp \
      --storage-account mystorageaccount \
      --os-type linux
   ```
2. Local Development
    ```
   # Install dependencies
    pip install -r requirements.txt
    
    # Run locally
    func start
    
    # Test locally
    curl "http://localhost:7071/api/hello_function?name=Azure"
   ```

3. Deploy to Azure
    ```
   func azure functionapp publish myFunctionApp
   ```

## Test Deployed Function

```bash
curl "https://myFunctionApp.azurewebsites.net/api/hello_function?name=Production"
```
Expected response: Hello, Production!

## Troubleshooting Commands

```bash
# Restart Function App
az functionapp restart --name myFunctionApp --resource-group myResourceGroup

# Check app settings
az functionapp config appsettings list --name myFunctionApp --resource-group myResourceGroup

# Add required setting for function visibility
az functionapp config appsettings set --name myFunctionApp --resource-group myResourceGroup --settings "AzureWebJobsFeatureFlags=EnableWorkerIndexing"
```

## License

[MIT License](LICENSE)