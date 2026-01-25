Day 23: Automating User Data Configuration Using the CLI
The Nautilus DevOps Team is working on setting up a new virtual machine (VM) to host a web server for a critical application. The team lead has requested you to create an Azure VM that will serve as a web server using Nginx. This VM will be part of the initial infrastructure setup for the Nautilus project. Ensuring that the server is correctly configured and accessible from the internet is crucial for the upcoming deployment phase.

As a member of the Nautilus DevOps Team, your task is to create a VM using Azure CLI with the following specifications:

**Instance Name**: The VM must be named **xfusion-vm**.

**Image**: Use any available Ubuntu image to create this VM.

**Custom Script Extension/User Data**: Configure the VM to run a custom script during its launch. This script should:

- Install the Nginx package.
- Start the Nginx service.

**Network Security Group (NSG)**: Ensure that the VM allows HTTP traffic on port 80 from the internet.

Instructions:

1. Use Azure CLI commands to set up the VM in the specified configuration.
2. Ensure the VM is accessible from the internet on port 80.
3. The Nginx service should be running after setup.


Use the Azure CLI commands to complete the task.

```bash
# get the RG name
az group list -o table


# create a cloud-init.txt file

cat<<EOF>cloud-init.txt
#cloud-config
package_upgrade: true
packages:
  - nginx
runcmd:
  - systemctl start nginx
  - systemctl enable nginx
EOF

#  Create the Ubuntu VM with Cloud-Init
az vm create --resource-group kml_rg_main-1ee4cdaae7d046da --location eastus --name xfusion-vm   --image Ubuntu2204   --admin-username azureuser   --generate-ssh-keys   --custom-data cloud-init.txt   --size Standard_B1s --storage-sku Standard_LRS

#list resource
az resource list --resource-group kml_rg_main-1ee4cdaae7d046da -o table

#  Open Port 80 to allow HTTP traffic from the internet
az vm open-port \
  --resource-group kml_rg_main-ee1cc95c80854f1d \
  --name xfusion-vm \
  --port 80 \
  --priority 101

# open network security group
az network nsg list --resource-group kml_rg_main-1ee4cda

# get the rule of the group
az network nsg rule list --resource-group kml_rg_main-1ee4cdaae7d046da --nsg-name xfusion-vmNSG -o table
```

delete everything related to vm
```bash
 az vm delete   --resource-group KML_RG_MAIN-9AA6E830387340E4   --name xfusion-vm   --force-deletion true   --yes