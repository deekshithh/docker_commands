## Docker Container

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
