# Notes on Docker

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
you get the volume id by executing command docker volume ls

### To prune the unused docker volumes
```
docker volume prune
```

### To remove the docker volume
```
docker volume rm <volume id>
```

### Sample code for defining the docker volume in docker compose file

`````
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
````

### To tag the Volume to a container

#### First create a volume
```
docker volume create jenkins_home
```

### then map a volume during container initialization

```
docker run -d  --name devtest -v myvol2:/app nginx:latest
```
