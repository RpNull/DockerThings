# Docker Zero->Hero

## Docker Defined:
>Docker provides the ability to package and run an application in a loosely isolated environment called a container. The isolation and security allow you to run many containers simultaneously on a given host. Containers are lightweight and contain everything needed to run the application, so you do not need to rely on what is currently installed on the host. You can easily share containers while you work, and be sure that everyone you share with gets the same container that works in the same way.

Docker is a containerization platform, designed for rapid and scalable deployment of apps and infrastructure. 

Image:
>A read-only template containing instructions for building a container. Each set of instructions is packaged as a 'layer'. Images can be built and modified with a Dockerfile. Images are hosted in Repositories, and can be 'pulled' or 'pushed' remotely or locally.

This can be compared to a VM's OVA file.
An example of a Dockerfile can be found [here.](examples/Dockerfile)\
The example Dockerfile is being used to install several pythons scripts, as well as the python script dependencies into the base image.

Containers:
>A runable instance of an image. By default, containers are NONPERSISTANT, meaning any changes made inside of a running container will be lost when that container is killed or restarted. Persistant storage is established utilizing Volumes.

Volumes:
>A mapping of Host to Container file systems, allowing changes made to persist across container restarts. Docker allows for two forms of volumes: Named, and Bind.

A named volume is used for simple persistence, where data is just dumped to the drive. \
A Bind volume allows for targeted mountpoints, allowing us to modify and persist data. For example, you can mount the `/etc/hosts` file of your container, and add specific entries to it that you want to save and later modify.


## Docker Orchestration Overview

Multi-Container Applications:
>Docker applications are designed to be lightweight and portable, with each container possessing a specific purpose. If a containers purpose is to be a web server, it should be only that. A container should not host a web server, a proxy, and a database all in one. To simplify the meshing of these multicontainer applications, use Docker-Compose.

Docker-Compose:
>A multi-contianer application orchestration tool, that takes templates in the form of a YAML file. The YAML file will define the images, volumes, and networks to be established, and create all the required containers and processes to facilitate the application.

An example of a Docker-Compose file can be found [here.](examples/Docker-Compose.yaml).\
This Docker-Compose file starts all the required containers required to host a local Gitea instance. 

Docker Swarms
>Multiple Applications (or Services) can be hosted and managed by a single machine, or a group of machines (or nodes), also called a cluster. This is accomplished by establishing a Swarm. A Swarm consists of up to seven Manager nodes, and each Manager can have multiple Worker nodes. The Manager node assignes tasks to Workers. By default, all Manager nodes are also Worker nodes. Worker nodes are able to join and leave the Swarm at any point, and the Manager will conduct automated Load-Balancing accordingly.

## Docker Cheatsheet
Docker commands should be executed using Sudo.\
These are just the most commonly used commands, check the man pages for more.

Image Management:
```
docker image ls                       -List all currently pulled images
docker pull repository/imagename:tag  -Download an image to your host
ex: docker pull centos:latest
docker save imagename:tag -o filename   -Save an image to a tar file
docker load -i filename                 -Load an image from a tar file
docker image rm imagename:tag           -Remove a specified image
docker image prune                      -Remove all unused images

docker build -t Name:Tag /path/to/Dockerfile  -Build an image from a dockerfile.
```

Container Management:
```
docker ps   --> See active running containers. 
                        [ -a to see ALL containers ]
                        [ --no-trunc to see full output ]

docker logs CONTAINERID  -->Prints the logs from within the container, useful for troubleshooting.
docker exec -it containerid /bin/bash    -->Enter an interactive shell within the container.
docker export containerid           --> save a containers filesystem as a tar archive.
docker import filename              --> import a tarball as an image.
docker start/stop/restart/pause containerid
```

Docker-Compose
```
docker-compose up Docker-Compose.yml -d
docker-compose down Docker-Compose.yml          Start && Stopping via docker compose
```

Service Management:
```
docker service ls           -List all services
docker service logs SERVICEID   -Pull the logs from the service. Useful for troubleshooting
docker service inspect         -See detailed information about the service.
docker service update          -Update all containers running under the service.
docker serivce ps SERIVCEID --no-trunc  
```

Network Management:
```
docker network ls       -List all Networks
docker network inspect ID   -Detailed information of the network
docker network prune    -Remove unused networks
```

Swarm Management:
```
docker swarm init  ---> Initialize a swarm as a Manager Node. 
docker swarm join-token worker --> Display the token used to join the swarm as a worker node
docker swarm join-token manager --> Display the token used to joint he swarm as a manager node.
docker swarm join --token TOKENSTRING --> join the swarm
```