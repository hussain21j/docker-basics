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
