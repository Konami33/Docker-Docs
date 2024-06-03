# Documentation for Deploying a Monitored NGINX Web Server Using Docker

## Overview

This documentation provides step-by-step instructions for deploying an NGINX web server using Docker, as well as setting up monitoring. The deployment includes two Docker containers: one for NGINX and one for a monitoring agent. The instructions cover creating both detached and interactive containers, listing containers, viewing logs, and stopping and restarting containers.

## Architecture

The architecture consists of three containers:
1. **NGINX Container**: Runs the NGINX web server.
2. **Agent Container**: Monitors the NGINX server.

## Steps to Deploy the Monitored Web Server

### 1. Creating and Starting the NGINX Container

The first step is to create and start an NGINX container.

#### Command
```sh
docker run --detach \
--name web nginx:latest
```

#### Description
- **--detach**: Runs the container in the background.
- **--name web**: Names the container "web".
- **nginx:latest**: Specifies the NGINX image from Docker Hub.

#### Output
The command will output a unique identifier for the container.

![alt text](nginx-image.PNG)
 

### 2. Running the Monitoring Agent in an Interactive Container

The final step is to run the monitoring agent in an interactive container.

#### Command
```sh
docker run --interactive --tty \
--link web:web \
--name web_test \
busybox:latest /bin/sh
```

#### Description
- **--interactive (-i)**: Keeps stdin open even if not attached.
- **--tty (-t)**: Allocates a virtual terminal.
- **--link web:web**: Links the container to the NGINX container.
- **--name web_test**: Names the container "web_test".
- **busybox:latest /bin/sh**: Runs a shell program inside the container.

#### Testing the NGINX Server
Inside the interactive container, run:
```sh
wget -O - http://web:80/
```
You should see a "Welcome to NGINX!" message, indicating the server is running.

![alt text](nginx-02.png)


#### Detaching from the Interactive Container
Once the agent starts printing "System up," detach by pressing `Ctrl + P` followed by `Ctrl + Q`.

## Managing Containers

### Listing Containers
To list all running containers:
```sh
docker ps
```

### Viewing Container Logs
To view logs for a specific container:
```sh
docker logs <container_name>
```

### Stopping and Restarting Containers
To stop a container:
```sh
docker stop <container_name>
```

To restart a container:
```sh
docker start <container_name>
```

### Reattaching to a Container
To reattach to a running container:
```sh
docker attach <container_name>
```

### Detaching from an Attached Container
To detach from a container without stopping it, press `Ctrl + P` followed by `Ctrl + Q`.

## Conclusion

By following these steps, you have successfully deployed an NGINX web server using Docker and set up monitoring for the server. This setup ensures that your client's web server is closely monitored.