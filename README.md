## What is Docker?

Docker is a platform to develop, deploy, and run applications with containers. The use of Linux containers to deploy applications is called containerization.

A container is launched by running an image. An image is an executable package that includes everything needed to run an application--the code, a runtime, libraries, environment variables, and configuration files.

#Difference between containers and vms.

The key is that the underlying architecture is fundamentally different between the containers and virtual machines. The analogy we
use here at Docker is comparing houses (virtual machines) to apartments (Docker containers).

Houses (the VMs) are fully self-contained and offer protection from unwanted guests. They also each possess their own infrastructure – plumbing, heating, electrical, etc. Furthermore, in the vast majority
of cases houses are all going to have at a minimum a bedroom, living area, bathroom, and kitchen. It’s incredibly difficult to ever find a “studio house” – even if one buys the smallest house they can find, they may end up buying more than they need because that’s just how houses are built.

Apartments (Docker containers) also offer protection from unwanted guests, but they are built around shared infrastructure. The apartment building (the server running the Docker daemon, otherwise known as a Docker host) offers shared plumbing, heating, electrical, etc. to each apartment. Additionally apartments are offered in several different sizes – from studio to multi-bedroom penthouse. You’re only renting exactly what you need.

Docker containers share the underlying resources of the Docker host. Furthermore, developers build a Docker image that includes exactly what they need to run their application: starting with the basics and adding in only what is needed by the application.


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
