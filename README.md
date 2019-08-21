## What is Docker?

Docker is a platform for developers and sysadmins to develop, deploy, and run applications with containers. The use of Linux containers to deploy applications is called containerization.

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
The **RM** requires a container ID to remove a specific conainer. It permanently deletes the container from the system.

```
docker container rm [container_id]
```


```
docker container stop [container_id]
```


```
docker container start cool_mclaren
```


```
docker containers logs mynginx
docker container logs --tail 100 mynginx
docker container logs --details cool_mclaren
docker container logs --follow cool_mclaren
```

# Get container details.

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

# Getting inside a shell in containers.

Start new ubuntu container interactively. Once excuted the command will open an interactive ubuntu command line. Similar to SSH.
```
docker container run -it --name ubuntu ubuntu
```

Start the existing container with interactive mode.
```
docker container start -ai ubuntu
```

Run additional command in running containers.
```
docker container exec -it ubuntu bash
```

Instead of ubntu we can use another ubuntu distribution called alpine. Which is light weight.

```
docker pull alpine
```
