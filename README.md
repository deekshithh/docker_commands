#Docker key commands
This repository lists some of the key commands
```shell
docker container ls
docker container ls -a
docker container rm [container_id]
docker container run --publish 80:80 nginx**
docker container run --publish 80:80 --detach nginx**
docker container stop [container_id]
docker containers logs cool_mclaren
docker container logs tail -100f cool_mclaren
docker container logs --details cool_mclaren
docker container logs --follow cool_mclaren
docker container top cool_mclaren
docker container start cool_mclaren
docker container run --publish 3306:3306 --detach --env MYSQL_RANDOM_ROOT_PASSWORD=yes mysql
```
