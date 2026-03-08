As per recent requirements shared by the Nautilus application development team, they need custom images created for one of their projects. Several of the initial testing requirements are already been shared with DevOps team. Therefore, create a docker file /opt/docker/Dockerfile (please keep D capital of Dockerfile) on App server 2 in Stratos DC and configure to build an image with the following requirements:



a. Use ubuntu:24.04 as the base image.


b. Install apache2 and configure it to work on 6400 port. (do not update any other Apache configuration settings like document root etc).

c. The container should be running and apache2 should be up and running when the container is started using the image built from this docker file. (Hint: use CMD instruction to start apache2 in foreground mode).
To create a Dockerfile that meets the specified requirements, you can follow the steps below:
1. Connect to App Server 2 in Stratos Datacenter using SSH.
2. Create the Dockerfile in the specified location using a text editor like nano or vim:
   ```
   nano /opt/docker/Dockerfile
   ```
3. Add the following content to the Dockerfile:
   ```bash
    # Use ubuntu:24.04 as the base image
    FROM ubuntu:24.04
    # Install apache2
    RUN apt update && apt install apache2 -y
    # Configure Apache to listen on port 6400
    RUN sed -i 's/Listen 80/Listen 6400/' /etc/apache2/ports.conf && \
        sed -i 's/<VirtualHost \*:80>/<VirtualHost \*:6400>/' /etc/apache2/sites-available/000-default.conf
    # Expose port 6400
    EXPOSE 6400
    # Start apache2 in foreground mode
    CMD ["apachectl", "-D", "FOREGROUND"]
    ```
4. Save the file and exit the text editor.
5. Build the Docker image using the following command:
   ```
   docker build -t custom-apache:latest -f /opt/docker/Dockerfile .
   ```