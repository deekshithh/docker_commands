## Docker Images

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

#### Keep Your Docker System Clean

You can use **prune** commands to clean up images, volumes, build cache, and containers.

```
docker system df
```

```
docker image prune
```

```
docker system prune -a
```