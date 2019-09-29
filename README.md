## What is Docker?

Docker is a platform to develop, deploy, and run applications with containers. The use of Linux containers to deploy applications is called containerization.

A container is launched by running an image. An image is an executable package that includes everything needed to run an application--the code, a runtime, libraries, environment variables, and configuration files.


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

