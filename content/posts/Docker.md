---
title: "Docker"
date: 2019-01-13T20:36:15+09:00
draft: true
---

# Docker Projects
1. Docker Engine
2. Docker Swarm
3. Docker Machine
4. Docker Compose
5. Private Registry
6. Kitematic

# Start Docker
```
$ docker run -i -t ubuntu:14.04
```
Type `exit` on container shell, or `Ctrl + D`. This stops the container
`Ctrl + P, Q` does not stops container
```
docker pull centos:7
docker images
docker inspect CONTAINER_NAME
```
run command : pull + create + start + attach (if there are -i -t option)
create command : pull + create

```
docker ps -a
docker stop CONTAINER_NAME
docker rm (options) CONTAINER_NAME
docker container prune
docker stop $(docker ps -a -q)
docker rm $(docker ps -a -q)
```

```
docker run - it --name mywebserver -p 8080:80 ubuntu:14.04 (HOST_PORT:CONTAINER_PORT)
docker run - it --name mywebserver -p 192.168.0.100:8080:80 ubuntu:14.04 
docker run - it --name mywebserver -p 80 ubuntu:14.04 (random host port)
```

```
apt-get update
apt-get install apache2 -y
service apache2 start
```

# Container Application
```
docker run -d \
--name wordpressdb \
-e MYSQL_ROOT_PASSWORD=password \
-e MYSQL_DATABSE=wordpress \
mysql:5.7

docker run -d \
-e WORDPRESS_DB_PASSWORD=password \
--name wordpress \
--link wordpressdb:mysql \
-p 80 \
wordpress
```



# Resource
https://qiita.com/gold-kou/items/44860fbda1a34a001fc1
https://www.docker.com/resources/what-container