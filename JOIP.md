14-04-20223
-----------
1. Run hello-world docker container  and observe the container status
* ` docker container run -d --name helloworld hello-world`
* `docker container ls`
* `docker container ls -a`
![preview](images/dkr1.png)
* status is container is not running
  ![preview](images/dkr2.png)
2. Check the docker images and also write down the size of hello-world image
* `docker image ls`
![preview](images/dkr3.png)
3. Run the nginx container with name as nginx1 and expose it on 8080 port on docker host
* `docker container run -d -p 8080:80 --name nginx1 nginx`
![preview](images/dkr4.png)
![preview](images/dkr5.png)

4. Explain docker container lifecycle
* There are different stages in docker, when we create docker contianers, which is known as Docker container lifecycle
* The stages are
  * **Created**
  * **Running**
  * **Paused**
  * **Stopped**
  * **Deleted** 
![preview](images/dkr6.png)
#### Create Containers
* using `docker container create` command will create a container with specified `docker image`
* `docker container create --name <container name> <imagename>:<tag>`
![preview](images/dkr7.png)
* here if we observe `create` command only creates container ,it will not run the container
* if we run `docker container ls` it will not be shown.
* if we run `docker container ls -a` then only it will be shown.
* status : is created , not running
#### Start Containers
* using `docker container start` command we will start the already created container
* `docker container start <container name>`
![preview](images/dkr8.png)
* here the status of container is up and running
* if we run `docker container ls` it will shown
#### Run Containers
* using `docker container run` command will do work of both `create` and `start` commands
* it will create container and also up and running
* `docker container run -d -P --name <container name> <image name>:<tag>`
![preview](images/dkr9.png)
* so by using run command : it will create and start the container
#### Pause Containers
* using `docker container pause` command ,we will pause the processes of container
* `docker container pause <container name>` 
![preview](images/dkr10.png)
* here it will pause our application loading ,but it will be visible in `docker container ls`
* now we can again run container by using `unpause`
![preview](images/dkr11.png)
#### Stop Container
* using `docker container stop` command we can stop the running container
* `docker container stop <container name>`
![preview](images/dkr12.png)
* it will completly stop container
* if we want we can again start containe by `start` command
#### Delete Container
* using `docker container rm` we can delete the containers
* `docker container rm <container name>`
* from above we can delete container which is stopped, running containers cannot be delete by this
![preview](images/dkr13.png)
* running conatiners can be deleted by using `force -f` option
* `docker container rm -f <container name>`
  
5. Explain what happens when you run the docker container




