# Notes on Docker

# Docker 

## Docker commands

### List the running containers
```
docker ps --all
```

### List all the docker images available in local

```
docker images
```

### Build the docker image with Dockerfile with filename Dockerfile

```
docker build -t <image/name> .

-t -- to tag name to image 
```

#### if the filename is not Dockerfile, then use the below command

```
docker build -t <image/name> -f <filname> .
```

### Create a container using the image

```
docker create -n konman/jenkins <image name>
-n will tag name to a container created

```

container id will be created

### Start a container
```
docker start <container id>
```

### Stop running container
```
docker stop <containerId/ container-name>
```

### Directly run a container

```
docker run -p 3000:3000 -n konman-name <image-name>
```

### Prune all the images

```
docker system prune -a
```


## Docker Compose

### What is Docker Compose?
Compose is a tool for defining and running multi-container Docker applications

Example for Docker Compose
```
version: '3'
# Define services
services:
  # Service Jenkins
  jenkins:
  	# Build to define the Dockerfile to use for the container
    build: 
      dockerfile: Dockerfile
      context: ./jenkins
    # To expose ports inside the network
    expose:
      - 5000
    # to expose the ports in the network and host
    ports:
      - "8080:8080"
      - "5001:5000"
    # to define the volumes
    volumes:
      - jenkins_home:/var/jenkins_home
    restart: on-failure

# Volumes created
volumes:
  jenkins_home:
```

### To start the containers define the docker compose, execute the below command in the directory where docker-compose.yml file present
```
docker compose up
```

### To stop the containers defined in the docker compose, execuet the below command in the directory where docker-compose.yml file present
```
docker compose down
```

### if the docker compose filename is different, then start the container with the below command

```
docker compose -f <fileName> build
```

## Difference Between Publish and Ports in Docker

The expose section allows us to expose specific ports from our container only to other services on the same network. We can do this simply by specifying the container ports.
The ports section also exposes specified ports from containers. Unlike the previous section, ports are open not only for other services on the same network, but also to the host.

## Difference between Volume and Mount

With Bind Mount, a file or directory on the host machine is mounted into a container. The file or directory is referenced by its full or relative path on the host machine.
With Volume, a new directory is created within Docker's storage directory on the host machine, and Docker manages that directory's content.


## Command related to Docker Volumes

### To create docker volume called jenkins_home
```
docker volume create jenkins_home
```

### To list all the docker volumes
```
docker volume ls
```

### To get the detaile of docker volume
```
docker volume describe <volume id>
```

####
you get the volume id by executing command 
```
docker volume ls
```

### To prune the unused docker volumes
```
docker volume prune
```

### To remove the docker volume
```
docker volume rm <volume id>
```

### Sample code for defining the docker volume in docker compose file

```
version: '3'
services:
  jenkins:
    build: 
      dockerfile: Dockerfile
      context: ./jenkins
    expose:
      - 5000
    ports:
      - "8080:8080"
      - "5001:5000"
    volumes:
      - jenkins_home:/var/jenkins_home
    restart: on-failure

volumes:
  jenkins_home:

```

### To tag the Volume to a container

#### First create a volume
```
docker volume create jenkins_home
```

### then map a volume during container initialization

```
docker run -d  --name devtest -v myvol2:/app nginx:latest
```
