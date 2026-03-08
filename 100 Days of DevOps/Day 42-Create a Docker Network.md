The Nautilus DevOps team needs to set up several docker environments for different applications. One of the team members has been assigned a ticket where he has been asked to create some docker networks to be used later. Complete the task based on the following ticket description:


a. Create a docker network named as ecommerce on App Server 2 in Stratos DC.


b. Configure it to use bridge drivers.


c. Set it to use subnet 172.28.0.0/24 and iprange 172.28.0.0/24.

To create a Docker network with the specified requirements, you can follow the steps below:
1. Connect to App Server 2 in Stratos Datacenter using SSH.
2. Use the following command to create a Docker network named "ecommerce" with the specified configurations:
   ```bash
    docker network create ecommerce \
    --driver bridge \
    --subnet 172.28.0.0/24 \
    --ip-range 172.28.0.0/24
    ```
3. Verify that the network has been created successfully by listing all Docker networks:
    ```bash
    docker network ls
    ```
4. You should see the "ecommerce" network listed with the correct configurations.
By following these steps, you will have successfully created a Docker network named "ecommerce" with the bridge driver, and configured it to use the specified subnet and IP range.