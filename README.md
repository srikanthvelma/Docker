# Docker Notes and Practice
### Docker Installation
* docker installation from docker script [referhere](https://get.docker.com/)
```sh
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
sudo usermod -aG docker < username >
exit and relogin
docker info
```
* docker commands cheat sheet refer[https://dockerlabs.collabnix.com/docker/cheatsheet/]
* official docs for `docker commands` [refer][https://docs.docker.com/engine/reference/commandline/docker/]  
  
### docker Images
* We have official docker images, which are available in **docker hub**.
* We can pull docker images from the docker hub.
* General naming convention for images is 
```sh
[username]/[repository]:[tag]
Ex:  srikanthvelma/spc:1.0.0
dafault tag is **latest**
official images will does not have username
```
Ex: to pull the official images
```sh
docker image pull nginx:1.23
docker image pull alpine:3.16
docker image pull jenkins/jenkins
```
#### docker image commands
* `docker image < COMMAND >` --- Manages images
* `docker images build`  --- Build an image from docker file
* `docker image ls` --- show the images list
* `docker image ls -a -q` --- show the list of all images(-a) and only ids(-q) 
* `docker image pull` --- pulls the docker image from a registry
* `docker image push` --- pushes the image to registry
* `docker image rm` --- removes the images
* `docker image rm $(docker image ls -a -q)` --- removes all images


#### docker container commands
* `docker container <COMMAND>` --- manages containers
* `docker container ls`
* `docker container run commands`
``` 
1. docker container run <image name>:<image tag>
2. docker container run -d --name <containername> <image name>:<image tag>
3. docker container run -d -p:30001 --name <containername> <image name>:<image tag>
```
* `docker container ls -a -q   (-a all) (-q quiet only id)`
* `docker container start <name or id>`
* `docker container stop <name or id>`
* `docker container rm <name or id>` 
* with this if container is running it will delete,so we have stop and rm or use below force delete command
* `docker container rm -f <name or id>`    (force delete)
* `docker container rm -f $(docker container ls -a -q)`   (delete all at a time)
* docker container run with port forwarding
* `docker container run -d -p <host-port>:<container port> --name <container name> <image name>:<image tag>`
* if we give `-P` capital P ,host port will assigned by docker
* `docker container run -d -P --name <container name> <image>`
* `docker container pause <contaner name>`
* `docker container unpause <contaier name>`
* `docker container -it -p 30000:8080 amazoncorretto:17 myspc17 /bin/bash   --- (interactive mode)`
* if you have already a running container we can create image out of it by `docker container commit`
* `docker container commit <container name> <imagename>:<tag>`
* `docker container run -d -p <host-port>:<container port> --name <containername> <imagename>:<tag> <any command>`
```
docker container run -d -p 30001:8080 --name spc1 myspc:latest java -jar spring-petclinic-2.4.2.jar
```
### Docker File
#### Basic Instructions
* **FROM** : download basic image required for it
* **RUN** : used to run any required commands to be executed
* **EXPOSE** : port number
* **CMD** : this is the command to run the application at last
* example docker file for scp
* 1. by using amazoncorretto:17
```Dockerfile
FROM amazoncorretto:11
RUN curl https://srikanthcicd.s3.ap-south-1.amazonaws.com/spring-petclinic-3.0.0-SNAPSHOT.jar
EXPOSE 8080
CMD ["java", "-jar", "spring-petclinic-3.0.0-SNAPSHOT.jar"]
```
* `docker image build -t myspc:corretto17 .`
* `docker container run -d -p 30001:8080 --name myspc1 myspc:corretto17`
* 2. start from os - `alpine`
```Dockerfile
FROM alpine:3
RUN apk add openjdk17
RUN wget https://srikanthcicd.s3.ap-south-1.amazonaws.com/spring-petclinic-3.0.0-SNAPSHOT.jar
EXPOSE 8080
CMD ["java", "-jar", "spring-petclinic-3.0.0-SNAPSHOT.jar"]
```
* `docker image build -t myspc:alpine .`
* `docker container run -d -p 30002:8080 --name myspc2 myspc:alpine`
* **LABEL** : this adds meta data - like author,org,project etc
* **ADD** : instruction can add the files into docker image from local file system as well as from http
* **COPY**: supports only local file system
* we can use ADD to download the spc jar file into docker image
```Dockerfile
FROM amazoncorretto:11
LABEL author="srikanthvelma"
LABEL project="dockerlearining"
LABEL org="qt"
ADD https://srikanthcicd.s3.ap-south-1.amazonaws.com/spring-petclinic-3.0.0-SNAPSHOT.jar /spring-petclinic-3.0.0-SNAPSHOT.jar
EXPOSE 8080
CMD ["java", "-jar", "spring-petclinic-3.0.0-SNAPSHOT.jar"]
```
* `exec` allow us to run any linux command
* `docker container exec <container name> <command>`
* ex: `docker container exec nginx1 printenv`
* `docker container exec -it <c-name> <shell>` allow us to login in to container
* `docker container exec -it nop1 /bin/bash`

* **ENV** : we can define environmental variables by this
* **ARG** : we define parameters by using this

#### what is detached mode (-d)
* Running container normally will take us to attached mode.
* In detached mode container executes and gives us back the access to terminal.
* Once we start the container in detached mode we can still view the STDOUT and STDERR by executing `docker container attach <container-name-or-id>`.
* To exit from attach mode `Ctrl+PQ`
#### Docker container will be in running state as long as command in cmd is running
* Ex: if we give cmd in docker file like `CMD ["sleep","10s"]
* so in above sys will sleep for 10s and container will moved to excited stateonce the command in CMD has finished executing.
### Docker file for nop,gol
### Docker Image layers
* Docker image is collection of layers and some metadata
* Docker image gets first set of layers from base image
* Any Additional changes w.r.t ADD/COPY creates extra layers
* Each RUN instruction which needs some storage creates layer
* It is recommended to use Multiple commands in RUN instruction rather than multiple RUN instructions as this leads to too many layers
* Docker has a filesystem which is aware of layers `overlay2`
#### Containers and layers
* When a container gets created all the effective read-only image layers are mounted as disk to the container
* Docker creates a thin read write layer for each container.
* Any changes made by container will be stored in this layer
* Problem: when we delete container read write layer will be deleted.
* [referhere][https://directdevops.blog/2019/09/26/docker-image-creation-and-docker-image-layers/] for the article on layers
* [referhere][https://directdevops.blog/2019/09/27/impact-of-image-layers-on-docker-containers-storage-drivers/] for layers and storage drivers
### Docker statefull applications and stateless
* Stateful applications use local storage to store any state
* Stateless applications use external systems (database, blobstorage etc) to store the state
* We need not do anything special if your application is stateless in terms of writable layer, but if it stateful we need to preserve the state.
#### Solving the Problem with Writable Layers
* lets create mysql db container form official `mysql` image
```
docker container run -d --name mysqldb -e MYSQL_ROOT_PASSWORD=rootroot -e MYSQL_DATABASE=employees -e MYSQL_USER=qtdevops -e MYSQL_PASSWORD=rootroot -P mysql:8
```
* To login into container
```
docker container exec -it mysqldb mysql --password=rootroot
```
#### Docker volumes
* Always ensure volumes are automatically created for the stateful applications as part of Dockerfile (VOLUME instruction)
* Volumes are of two types
    1. Explicity created (docker volume create myvol)
    2. automatically created as part of container creation
* Ensure we have knowledge on necessary folders where the data is stored and use volumes for it
* for more db containers and creating with volumes `refer JOIP task on 16-04-2023 in joip.md`
### Shell file to clean everything
```
#!/bin/bash
docker container rm -f $(docker container ls -a -q)
docker volume prune
docker image rm -f $(docker image ls -q)
```

### Docker Networks
* [referhere][https://directdevops.blog/2019/10/05/docker-networking-series-i/]
* 
