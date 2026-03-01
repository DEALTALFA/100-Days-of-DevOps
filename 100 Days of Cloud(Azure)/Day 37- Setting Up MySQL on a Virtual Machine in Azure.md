The Nautilus DevOps team is tasked with integrating a PHP application hosted on an Azure VM with a MySQL database hosted on another Azure VM. This will validate the application's ability to connect to the database in the cloud.

Create the MySQL VM:

Create a VM named devops-mysql-vm using the MySQL Jetware image from the Azure Marketplace.
Configure the VM in the Central US region.
Use Password as the authentication type.
Set the username as devops_admin and the password as Namin@123456.
Allow inbound traffic on port 3306 to enable MySQL access.
Setup the MySQL Database:

SSH into the devops-mysql-vm.
Use the sudo /jet/enter mysql command to access the MySQL shell.
Create a database named devops_db.
Create a MySQL user named devops_user with password password123.
Grant all privileges on the devops_db database to this user.
PHP VM Setup:

A VM named devops-php-vm already exists in the East US region.
This VM is hosting a PHP application and contains a pre-existing db_test.php file in the /var/www/html/ directory.
Database Connection Configuration:

Retrieve the public IP address of the devops-mysql-vm.
Update the database connection settings in the db_test.php file to use the MySQL credentials and public IP address of the devops-mysql-vm.
Validation:

Access the db_test.php file from the devops-php-vm using its public IP address.
Ensure the file displays the message Connected successfully, confirming the connection between the PHP application and the MySQL database.

Ensure the MySQL database allows inbound traffic on port 3306.
Verify that the PHP application on the devops-php-vm successfully connects to the MySQL database on the devops-mysql-vm

```bash
# Create the MySQL VM
az vm create --resource-group kml_rg_main-1ee4cdaae7d046da --name devops-mysql-vm --image MySQL-Jetware:latest --location centralus --admin-username devops_admin --admin-password Namin@123456 --size Standard_B1s --storage-sku Standard_LRS
# Allow inbound traffic on port 3306
az vm open-port --resource-group kml_rg_main-1ee4cdaae7d046da --name devops-mysql-vm --port 3306 --priority 102
# SSH into the MySQL VM
az vm ssh --resource-group kml_rg_main-1ee4cdaae7d046da --name devops-mysql-vm
# Access MySQL shell
sudo /jet/enter mysql
# Create the database and user
CREATE DATABASE devops_db;
CREATE USER 'devops_user'@'%' IDENTIFIED BY 'password123';
GRANT ALL PRIVILEGES ON devops_db.* TO 'devops_user'@'%';
FLUSH PRIVILEGES;
# Exit MySQL shell
exit
# Retrieve the public IP address of the MySQL VM
az vm show --resource-group kml_rg_main-1ee4cdaae7d046da --name devops-mysql-vm --show-details --query publicIps -o tsv
# Update the db_test.php file with the MySQL credentials and public IP address
# Assuming the public IP address is
MYSQL_VM_IP=<public_ip_address>
sed -i "s/<mysql-vm-public-ip>/$MYSQL_VM_IP/g" /var/www/html/db_test.php
sed -i "s/nautilus_user/devops_user/g" /var/www/html/db_test.php
sed -i "s/password/password123/g" /var/www/html/db_test.php
sed  -i "s/nautilus_db/devops_db/g" db_test.php
# Access the db_test.php file from the PHP VM using its public IP address
PHP_VM_IP=$(az vm show --resource-group kml_rg_main-1ee4cdaae7d046da --name devops-php-vm --show-details --query publicIps -o tsv)
curl http://$PHP_VM_IP/db_test.php
```
# Ensure the file displays the message Connected successfully, confirming the connection between the PHP application and the MySQL database.
```
# Cleanup resources
az vm delete --resource-group kml_rg_main-1ee4cdaae7d046da --name devops-mysql-vm --yes
az vm delete --resource-group kml_rg_main-1ee4cdaae7d046da --name devops-php-vm --yes
```