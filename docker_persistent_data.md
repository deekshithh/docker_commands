## Docker Persistent Data

Docker is immutable and short-lived.

Two ways to store the persestent data; volumes and Bind Mounts.

### Seperation of concerns

## Volumes
 Volumes make special locations outside of containers union file system(UFS).

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
docker inspect mysql-db


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
Link container path to host path

