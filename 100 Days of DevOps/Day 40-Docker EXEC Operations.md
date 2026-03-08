One of the Nautilus DevOps team members was working to configure services on a kkloud container that is running on App Server 3 in Stratos Datacenter. Due to some personal work he is on PTO for the rest of the week, but we need to finish his pending work ASAP. Please complete the remaining work as per details given below:


a. Install apache2 in kkloud container using apt that is running on App Server 3 in Stratos Datacenter.


b. Configure Apache to listen on port 5001 instead of default http port. Do not bind it to listen on specific IP or hostname only, i.e it should listen on localhost, 127.0.0.1, container ip, etc.


c. Make sure Apache service is up and running inside the container. Keep the container in running state at the end.

To complete the pending work, you can follow the steps below:
1. Connect to App Server 3 in Stratos Datacenter using SSH.
2. List the running containers to identify the container ID of the kkloud container using the command:
   ```
   docker ps
   ```
3. Once you have the container ID, access the kkloud container using the command:
   ```
    docker exec -it <container_id> bash
    ```
4. Inside the container, update the package list and install apache2 using the following commands:
    ```
    apt update
    apt install apache2 -y
    ```
5. After installing apache2, open the Apache configuration file using a text editor like nano or vim:
    ```
    nano /etc/apache2/ports.conf
    ```
6. Change the line that says `Listen 80` to `Listen 5001` and save the file.
7. Next, open the default Apache site configuration file:
    ```
    nano /etc/apache2/sites-available/000-default.conf
    ```
8. Change the line that says `<VirtualHost *:80>` to `<VirtualHost *:5001>` and save the file.
9. Restart the Apache service to apply the changes:
    ```
    service apache2 restart
    ```
10. Finally, check the status of the Apache service to ensure it is running:
    ```
    service apache2 status
    ```
11. Exit the container and ensure that it is still running:
    ```
    exit
    docker ps
    ```
By following these steps, you will have successfully installed and configured Apache to listen on port 5001 inside the kkloud container, and ensured that the service is up and running.
