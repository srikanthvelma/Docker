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
* 