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