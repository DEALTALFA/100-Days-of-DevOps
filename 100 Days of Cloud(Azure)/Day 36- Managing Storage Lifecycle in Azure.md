The Nautilus DevOps team needs to optimize data retention costs by automating the deletion of old blobs. They plan to implement Blob Lifecycle Management for a specific container in Azure Storage.

Task:
1) Create a Storage Account:

Name the storage account datacenterstor3199.
Set the region to East US.
Use Locally-redundant storage (LRS) as the redundancy option.
2) Create a Blob Container:

Name the container datacenter-container3199.
3) Upload a File to the Container:

Upload the file named tempfile.txt to the container. The file is present under /root of the client host.
4) Configure Blob Lifecycle Management:

Apply a Lifecycle Management rule named datacenter-del-rule to the container datacenter-container3199 to delete blobs after 7 days of last modification.
5) Validation:

Verify that the Lifecycle Management rule named datacenter-del-rule is correctly applied.

```bash
# Create a Storage Account
az storage account create --name datacenterstor3199 --resource-group kml_rg_main-1ee4cdaae7d046da --location eastus --sku Standard_LRS
# Create a Blob Container
az storage container create --account-name datacenterstor3199 --name datacenter-container3199
# Upload a File to the Container
az storage blob upload --account-name datacenterstor3199 --container-name datacenter-container3199 --name tempfile.txt --file /root/tempfile.txt
# Configure Blob Lifecycle Management
az storage account management-policy create --account-name datacenterstor3199 --policy '{
  "rules": [
    {
      "name": "datacenter-del-rule",
      "enabled": true,
      "type": "Lifecycle",
      "definition": {
        "actions": {
          "baseBlob": {
            "delete": {
              "daysAfterModificationGreaterThan": 7
            }
          }
        },
        "filters": {
          "blobTypes": ["blockBlob"],
          "prefixMatch": ["datacenter-container3199/"]
        }
      }
    }
  ]
}'
# Validation
az storage account management-policy show --account-name datacenterstor3199
# delete everything related to storage account
az storage account delete --name datacenterstor3199 --resource-group kml_rg_main-1ee4cdaae7d046da --yes
