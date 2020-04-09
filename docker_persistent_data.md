## Docker Persistent Data

Docker is immutable and short-lived.The data doesn’t persist when that container no longer exists, and it can be difficult to get the data out of the container if another process needs it.
A container’s writable layer is tightly coupled to the host machine where the container is running. You can’t easily move the data somewhere else.
Writing into a container’s writable layer requires a storage driver to manage the filesystem. The storage driver provides a union filesystem, using the Linux kernel. This extra abstraction reduces performance as compared to using data volumes, which write directly to the host filesystem.
Docker has two options for containers to store files in the host machine, so that the files are persisted even after the container stops: volumes, and bind mounts. This way docker features ensures the **separation of concerns**.

## Volumes
Volumes make special locations outside of containers union file system(UFS).
Volumes are stored in a part of the host filesystem which is managed by Docker (/var/lib/docker/volumes/ on Linux). Non-Docker processes should not modify this part of the filesystem. Volumes are the best way to persist data in Docker.
Removing a container doesn't remove the volume. Volumes require manual deletion.

```
docker container run -d --name mysql-test -e MYSQL_ALLOW_EMPTY_PASSWORD=true mysql
docker container inspect mysql-test

"Config": {
    "Image": "mysql",
    "Volumes": {
        "/var/lib/mysql": {}
    },
    "WorkingDir": "",
    "Entrypoint": [
        "docker-entrypoint.sh"
    ],
    "OnBuild": null,
    "Labels": {}
}

```

```
docker volume ls

DRIVER              VOLUME NAME
local               e70635d8e13001e4fd51bf4dac8e8e2ba2449b8014fdb4246900a21ce696335d
```

```
docker volume inspect e70635d8e13001e4fd51bf4dac8e8e2ba2449b8014fdb4246900a21ce696335d

[
    {
        "CreatedAt": "2019-10-02T13:19:47Z",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/e70635d8e13001e4fd51bf4dac8e8e2ba2449b8014fdb4246900a21ce696335d/_data",
        "Name": "e70635d8e13001e4fd51bf4dac8e8e2ba2449b8014fdb4246900a21ce696335d",
        "Options": null,
        "Scope": "local"
    }
]

```

**Named volumes**
```
docker container run -d --name mysql-test -v mysql-db:/var/lib/mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=true mysql
```

```
docker volume inspect mysql-db


[
    {
        "CreatedAt": "2019-10-02T13:42:15Z",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/mysql-db/_data",
        "Name": "mysql-db",
        "Options": null,
        "Scope": "local"
    }
]

```
**Creating Volumes**
```
docker volume create mysql-data
```

```
docker volume inspect mysql-data

[
    {
        "CreatedAt": "2019-10-02T14:36:16Z",
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "/var/lib/docker/volumes/mysql-data/_data",
        "Name": "mysql-data",
        "Options": {},
        "Scope": "local"
    }
]
```


## Bind Mounts
Bind Mounts link container path to host path. Bind mounts may be stored anywhere on the host system. They may even be important system files or directories. Non-Docker processes on the Docker host or a Docker container can modify them at any time.

```
pwd

/Users/deekshith.h/project/bind-mount
```

```
touch readme.txt
```

```
docker container run -d -p 80:80 -v /Users/deekshith.h/project/bind-mount:/usr/share/nginx/html deek-nginx

docker container run -it -v $(pwd):/new_folder  ruby:2.7.1-alpine /bin/sh
```

```
docker container exec musing_carson ls

Dockerfile
index.html
readme.txt
```

