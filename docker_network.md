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

#### DNS Round Robin Test

Round-robin DNS is a load balancing technique where the balancing is done by a type of DNS server called an authoritative nameserver, rather than using a dedicated piece of load-balancing hardware. Round-robin DNS can be used when a website or service has their content hosted on several redundant web servers; when the DNS authoritative nameserver is queried for an IP address, the server hands out a different address each time, operating on a rotation. This is particularly useful when the redundant web servers are geographically separated, making traditional load-balancing difficult. Round-robin is known for it’s ease of implementation, but it also has strong drawbacks.

We can implement DNS round robin in docker using **--net-alias** command.

For example, create a new network.
```
docker network create my_network
```

Run two elasticsearch containers in the same network with a network alias 'search'.
```
docker container run -d --network my_network --network-alias search elasticsearch:2
docker container run -d --network my_network --network-alias search elasticsearch:2
```

Using the alpine container we can check the ip addresses and the corresponding domain names of the containers in the network.
```
docker container run --rm --net my_network alpine nslookup search
```

When you curl the 'search' domain it will point to different elasticsearch containers during each request.
```
docker container run --rm --network my_network centos curl -s search:9200
```
