## What is Docker?

Docker is a platform for developers and sysadmins to develop, deploy, and run applications with containers. The use of Linux containers to deploy applications is called containerization.

A container is launched by running an image. An image is an executable package that includes everything needed to run an application--the code, a runtime, libraries, environment variables, and configuration files.

A container is a runtime instance of an image--what the image becomes in memory when executed (that is, an image with state, or a user process)

The following section lists some of the important docker commands with examples.

## Docker commands

**RUN** command is used to run a commnad in the container.
**PUBLISH** argument is used to publish the container port to the host. Here the port 80 of the nginx is published to the same host port.
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



```
docker container ls
docker container ls -a
```

```
docker container rm [container_id]
```


```
docker container stop [container_id]
```

```
docker containers logs cool_mclaren
docker container logs tail -100f cool_mclaren
docker container logs --details cool_mclaren
docker container logs --follow cool_mclaren
```

```
docker container top cool_mclaren
```

```
docker container start cool_mclaren
```


