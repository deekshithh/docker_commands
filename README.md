## What is Docker?

Docker is a platform to develop, deploy, and run applications with containers. The use of Linux containers to deploy applications is called containerization.

A container is launched by running an image. An image is an executable package that includes everything needed to run an application--the code, a runtime, libraries, environment variables, and configuration files.

A container is a runtime instance of an image--what the image becomes in memory when executed (that is, an image with state, or a user process)

The following sections list some of the important docker commands with examples.

## Docker start, stop and logs

**RUN** command is used to run a commnad in the container.
**PUBLISH** command publishes the container port to the host. Here the port 80 of the nginx is published to the same host port.
```
docker container run --publish 80:80 nginx
```

**DETACH** command runs container in background.
```
docker container run --publish 80:80 --detach nginx
```

**ENV** command is used to set the environment variables. While publishing the mysql container *MYSQL_RANDOM_ROOT_PASSWORD* environment variable comes handy.
```
docker container run --publish 3306:3306 --detach --env MYSQL_RANDOM_ROOT_PASSWORD=yes mysql
```

The command **LS** list all the containers that are currently running. Where as the **-a** along with **ls** list all the containers.
```
docker container ls
docker container ls -a
```

The **RM** requires a container ID to remove a specific conainer.
```
docker container rm [container_id]
```

If the container is running then the **rm -f** can be used to forcely delete the container.
```
docker container rm -f boring_soloe
```

**run --rm** command will help in cleaning up the container, once the command is executed.
```
docker container run --rm centos curl -s search:9200
```

The **STOP** command is used to stop the container.
```
docker container stop [container_id]
```

**START** command is used to start the existing container.
```
docker container start cool_mclaren
```

The **LOGS** command is used to access the container logs.
```
docker containers logs mynginx
docker container logs --tail 100 mynginx
docker container logs --details cool_mclaren
docker container logs --follow cool_mclaren
```

## Getting container details

List all the processes running on a single container.
```
docker container top cool_mclaren
```

Details of one container config.
```
docker container inspect mynginx
```

Live performance stats for all containers.
```
docker container stats mynginx
```

**PORT** command displays all the ports open in a container.
```
docker container port mynginx
```


## Getting inside a container

**run -it** will start a new ubuntu container interactively. Once excuted the command will open an interactive ubuntu command line. Similar to SSH.
```
docker container run -it --name ubuntu ubuntu
```

**start -ai** will start the existing container in interactive mode.
```
docker container start -ai ubuntu
```

**exec -it** will open the bash of running container.
```
docker container exec -it ubuntu bash
```

## Docker networks

The **ls** command for a network will list all the docker networks.
```
docker network ls
```

When the **netwok ls** command is executed for the first time, the following three default networks will be listed.

**Bridge**: The default network driver. If you don’t specify a driver, this is the type of network you are creating. Bridge networks are usually used when your applications run in standalone containers that need to communicate.

**host**: For standalone containers, remove network isolation between the container and the Docker host, and use the host’s networking directly. host is only available for swarm services on Docker 17.06 and higher.

**none**: For this container, disable all networking. Usually used in conjunction with a custom network driver. none is not available for swarm services.

**inspect** command is used to get information on a specific network.
```
docker network inspect bridge
```

**network create** command is used to create a new network. By default the newly created network will be attached to the **bridge** driver.
```
docker network create my_network
```

**--driver** command is used to attach the network to a particular driver.
```
docker network create my_network1 --driver bridge
```

While creating a new container it can be attached to a particular network using the **--network** command.
```
docker container run -d -p 80:80 --network my_network --name nginx_new nginx
```

**connect** command is used to connect an existing container to a network. One container can be connected to multiple networks.
```
docker network connect my_network nginx2
```

**disconnect** command is used to remove a container from the connected network.
```
docker network disconnect my_network nginx2
```

#### DNS

Containers shouldn't rely on IP's for inter-communication. Hence the Docker daemon has a built-in DNS server that containers use by default. Docker defaults the hostname to the container's name, but you can also set aliases.

For example, create two nginx containers and connect them to the same network.
```
docker container run -d -p 80:80 --network my_network --name nginx1 nginx
docker container run -d -p 8080:80 --network my_network --name nginx2 nginx
```

Get into one container and you should be able to call the other conatainer using the container name.
```
docker container exec -it nginx1 bash
apt-get update
apt-get install curl
curl nginx2
```

#### DNS Round Robin Test

Round-robin DNS is a load balancing technique where the balancing is done by a type of DNS server called an authoritative nameserver, rather than using a dedicated piece of load-balancing hardware. Round-robin DNS can be used when a website or service has their content hosted on several redundant web servers; when the DNS authoritative nameserver is queried for an IP address, the server hands out a different address each time, operating on a rotation. This is particularly useful when the redundant web servers are geographically separated, making traditional load-balancing difficult. Round-robin is known for it’s ease of implementation, but it also has strong drawbacks.

We can implement DNS round robin in docker using **--net-alias** command.

For example, create a new network.
```
docker network create my_network
```

Run two elasticsearch containers in the same network with a network alias 'search'.
```
docker container run -d --network my_network --network-alias search elasticsearch:2
docker container run -d --network my_network --network-alias search elasticsearch:2
```

Using the alpine container we can check the ip addresses and the corresponding domain names of the containers in the network.
```
docker container run --rm --net my_network alpine nslookup search
```

When you curl the 'search' domain it will point to different elasticsearch containers during each request.
```
docker container run --rm --network my_network centos curl -s search:9200
```

## Docker login
Imagine you made your own Docker image and would like to share it with the world you can sign up for an account on https://hub.docker.com/. After verifying your email you are ready to go and upload your first docker image.

The command **docker login** is used to Log in to a Docker registry.
```
docker login
```

The authentication details of all the logged in docker registries is stored in the **.docker/config.json** file.
```
cat ~/.docker/config.json
```

An user can logout from a registry using **docker logout** command.
```
docker logout
```

## Images

An Image is an ordered collection of root filesystem changes and the corresponding execution parameters for use within a container runtime. This specification outlines the format of these filesystem changes and corresponding parameters and describes how to create and use them for use with a container runtime and execution tool.

In simple words,
* App binaries and dependencies.
* Metadata about the image data and how to run the image.
* Not a complete OS. No kernel.
* Host provides the kernel. This is one of the major distinction between docker and VM.

**image ls** command is used to list all the images in the system.
```
docker image ls
```

**image pull** command pulls the specicific image from the docker hub.
```
docker image pull nginx:1.11.9-alpine
```
**image inspect** command displays detailed information on one or more images.
```
docker image inspect nginx:latest
```

#### Image layers
A Docker image consists of several layers. Each layer corresponds to certain instructions in your Dockerfile. The following instructions create a layer: RUN, COPY, ADD. The other instructions will create intermediate layers and do not influence the size of your image.

The command **docker history** shows the layers of changes made in a docker image.
```
docker history nginx:latest
```

#### Copy on write(COW)
When we launch an image, the Docker engine does not make a full copy of the already stored image. Instead, it uses something called the copy-on-write mechanism. This is a standard UNIX pattern that provides a single shared copy of some data, until the data is modified.

To do this, changes between the image and the running container are tracked. Just before any write operation is performed in the running container, a copy of the file that would be modified is placed on the writeable layer of the container, and that is where the write operation takes place. Hence the name, “copy-on-write”. [Read more..](https://blog.codeship.com/docker-storage-introduction/)

#### Image tag

***docker image** command retags the existing docker images. It creates a image with same image id and new repository name.
```
docker image tag nginx deekshithh/nginx:0.1
```

**image push** command is used to push the image to docker hub.
```
docker image push deekshithh/nginx
```

#### Building images and DockerFile

Creating a Dockerfile is as easy as creating a new file named “Dockerfile” with your text editor of choice and defining some instructions. The name of the file
does not really matter. Dockerfile is the default name but you can use any filename that you want (and even have multiple dockerfiles in the same folder)

The following are the ***Dockerfile Commands** used to create an image.

**ADD** – Defines files to copy from the Host file system onto the Container
```
    ADD ./local/config.file /etc/service/config.file
```

**CMD** – This is the command that will run when the Container starts
```
    CMD ["nginx", "-g", "daemon off;"]
```

**ENTRYPOINT** – Sets the default application used every time a Container is created from the Image. If used in conjunction with CMD, you can remove the application and just define the arguments there
```
    CMD Hello World!
    ENTRYPOINT echo
```

**ENV** – Set/modify the environment variables within Containers created from the Image.
```
    ENV VERSION 1.0
```

**EXPOSE** – Define which Container ports to expose
```
    EXPOSE 80
```

**FROM** – Select the base image to build the new image on top of
```
    FROM ubuntu:latest
```

**LABEL** maintainer – Optional field to let you identify yourself as the maintainer of this image. This is just a label (it used to be a dedicated Docker directive).
```
    LABEL maintainer=someone@xyz.xyz"
```

**RUN** – Specify commands to make changes to your Image and subsequently the Containers started from this Image. This includes updating packages, installing software, adding users, creating an initial database, setting up certificates, etc. These are the commands you would run at the command line to install and configure your application. This is one of the most important dockerfile directives.
```
    RUN apt-get update && apt-get upgrade -y && apt-get install -y nginx && rm -rf /var/lib/apt/lists/*
```

**USER** – Define the default User all commands will be run as within any Container created from your Image. It can be either a UID or username
```
    USER docker
```

**VOLUME** – Creates a mount point within the Container linking it back to file systems accessible by the Docker Host. New Volumes get populated with the pre-existing contents of the specified location in the image. It is specially relevant to mention is that defining Volumes in a Dockerfile can lead to issues. Volumes should be managed with docker-compose or “docker run” commands. Volumes are optional. If your application does not have any state (and most web applications work like this) then you don’t need to use volumes.
```
    VOLUME /var/log
```

**WORKDIR** – Define the default working directory for the command defined in the “ENTRYPOINT” or “CMD” instructions.
```
    WORKDIR /home
```

Example: create a Dockerfile.
```
FROM alpine:3.4

RUN apk update
RUN apk add vim
RUN apk add git
```

Run the **docker image build** command in the same folder to build the image.
```
docker image build -t deek-alpine .
```

The command **docker image build -f** is used to create am image from a specific Dockerfile.
```
docker image build -f deek.Dockerfile -t deek-alpine .
```

```
docker system df
```

You can use **prune** commands to clean up images, volumes, build cache, and containers.
```
docker image prune
```