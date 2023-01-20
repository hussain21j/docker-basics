# docker-basics
this is docker basics project

# docker-basics
this is docker basics project 

## Login to docker engine using ssh by putty
- default IP address is `192.168.99.100`
- default user name is `docker`
- default password is `tcuser`

## Docker container basic commands
```sh
# run and start a docker container for image nginx
$ docker container run --publish 80:80 nginx
# list of running containers
$ docker container ls
# list of all the containers
$ docker container ls -a
# run the docker in the background (detached from current terminal)
$ docker container run --publish 80:80 --detach nginx
# run the docker in the background with a name
$ docker container run --publish 80:80 --detach --name my-webhost nginx
# see the log of a particular docker container
$ docker container logs <container-name>  example : docker container logs my-webhost
# Stop a container 
$ docker container stop <container id / name>
# remove a container
$ docker container rm <container id / name>
# Force a running containe to stop and remove
$ docker container rm -f <container id / name>
# run mysql docker container with detached state and environment configuration as random password	
$ docker container run -d --name mysql -e MY_SQL_RANDOM_ROOT_PASSWORD=true mysql
# list the process 
$ docker container top nginx
# detailed information of the a container
$ docker container inspect nginx
# statistics of the container
# docker container stats
# runt the docker container in interective and command accepting mode
$ docker run -i -t busybox
# run container in detached state
# check all the running docker
$ docker ps
# list all the containers including the one which are not runnig
$ docker ps -a
# start a stopped container
$ docker start <container id or container name>
# docker detailed information
$ docker info
# detailed information of a container
$ docker inspect <container id>
# runnig tomcat on docker
$ docker run -it  -p host_port:container_port
# see the tomcat logs
$ docker logs <container id>
# Docker images
Images are layers , the lowest one is base image, each image is independent and docker pulls these one by one.
When conatiner is running there is anoter layer created called the writable container layer. when container gets off the writable
container layer is also gone
# go inside a running container
$ docker exec -it <container id or name> bash
```

## Docker network commands
```sh
# port on which the container is running
$ docker container port nginx
# networks that have been created
$ docker container port nginx
docker network ls
# The bridge network is the bridge between host OS and docker, by default all containers are attached to the host network

# detailed information of the network and containers attached to it
$ docker network inspect bridge

#create a new network
$ docker network create my_nginx_network 
#Running a container on a particular VPN
docker container run --publish 80:80 --detach --name my_nginx --network my_nginx_app nginx
# connect a running container to a network
$ docker network connect <network id> <container id>
# disconnect a container from a network
$ docker network disconnect <network id> <container id>
# removing a docker network (remember first you need to disconnect the container from this network)
$ docker network rm <network name or id>
```

## Docker image commands
```sh
# list of images available on local machine
$ docker image ls
# update an image to latest
$ docker pull <image name>
# download a particular version of an image
$ docker pull <image name>:<version number>  e.g. docker pull nginx:1.11.9
# layers inside image
$ docker image history <image name>
# detailed information of an image
$ docker image inspect <iamge name>
```

## Docker build 
### Docker build using the commit 
you can just made changes and commit those changes, now whenever you made up a continer of the image it will have the changes
### Docekr build 
its a more rubust solution prepare a file name Dockerfile
for example 
```sh
FROM openjdk:8u242-jre
ADD target/springboot-mysql-docker.jar springboot-mysql-docker.jar
EXPOSE 8086
ENTRYPOINT ["java", "-jar", "springboot-mysql-docker.jar"]
```
common commands 

```sh
# build image from a Dockerfile
#format docker build -t <image name> <path to Dockerfile>
```

## Dockerise a spring boot + mysql database
1. download the mysql database image https://hub.docker.com/_/mysql 
2. you can use the command `docker pull mysql` it will pull the command
3. bring up the mysql container with command for example
`docker run --name mysql-db -e MYSQL_ROOT_PASSWORD=password -e MYSQL_DATABASE=test -e MYSQL_USER=sa -e MYSQL_PASSWORD=password -d mysql`
4. configure database infromation in your spring boot application 
5. Build the image of spring boot application (for example using docker build)
6. bring up the spring boot app container (link with the database)
`docker run -p 8086:8086 --name user-app --link mysql-db -d user-app`


## Docker compose
doker compose is a tool for defining and running multiple containers. we can run multiple containers from a single file called docker-compose.yml file. and then with a single command we can start all containers 
first check if docker compose is installed using the command 
`docker-composer --version` or `docker-composer version`
sample docker-compose file for the spring boot and mysql application
```sh
version: '3'
services:
  user-app:
    #since the application expects the db to be available at the start time, so if fails then restart
    restart: on-failure
    build: .
    ports:
      - "8086:8086"
    depends_on:
      - mysql-db
  mysql-db:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: password
      MYSQL_DATABASE: test
      MYSQL_USER: sa
      MYSQL_PASSWORD: password
```
common docker compose commands

```sh
# to bring up the containers in from the docker-compose.yml file go to the directory where it is present. and use command
$ docker-composer up
# check the status of containers managed by docker compose
$ docker-compose ps
# aggregated logs of containers managed by docker compose
$ docker-compose logs
# logs of a specific container
$ docker-compose logs <container id or container name>
# stop all the conatainers is docker compose
$ docker-compose stop
# stop and remove all the containers in the docker compose
$ docker-compose rm -f
# rebuild all the images
$ docker-compose build
```



to bring down the containers and delete as well
`docker-compose rm -f`

