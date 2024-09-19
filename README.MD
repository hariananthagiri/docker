# DOCKER
* docker runs on linux servers not on windows 
* in windows we have a software called docker desktop but this software runs linux in background
* We have three types of methods in docker
1. physical server
    * booting time is very high
2. virtualisation
    * booting time is very less compired to physical server
    * we will use multiple guest os in one server
    * a big physical server will be there contain 16GB RAM, 1TB HD, CPU we will divide this big server into multiple logical servers
        * Host OS --> windows
        * VM Ware --> Hypervisor
    * Guest OS-1
        * 1GB RAM
        * Ubuntu
        * 100GB HD
    * Guest OS-2
        * 1GB RAM
        * Centos
        * 100GB HD
        * isolated from other servers
    * resource utilisation is good in VM
3. Containerization
    * we will have containers installed in VM...
    * containers are isolated from each other, system resources are shared...
    * containers take resources based on demand, it don't block resources
    * boot time is very less --> with in seconds your application is available
* **container/image** = Fat OS(4GB/3GB) + application run time + created users + created a directory + installed application
* **AMI(Amazon machine image)** = server + configured the server using ansible + stop server + take AMI
* **Docker Image** = Base OS(5MB-250MB) + application run time + created users + created a directory + installed application = max 500MB = immutable image = can take from DEV to PROD  
* Docker is portable
* **image is static ==> VM**
* **container is the running version of image**
* we will get all these images pulled from docker hub we have to signup to docker hub account  

### Docker installation
* create a server(centos) by loging in to aws account 
* go to chrome type docker install click on docker engine select centos

* **SYNTAX**
```
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo systemctl start docker
sudo systemctl enable docker
sudo systemctl status docker
```
* run the below commnd as narmal user you will get permission deneyed
```
docker ps
```
* it is because docker users can only use docker, when you install docker a group called as "docker" is created we have to add our user to that group
* we are using centos as of now our user name is centos add this user to docker group by using below command
```
usermod -aG docker centos
```
* after add centos to docker group logout once and login again then you can erun all the commands related to docker in centos

#### docker commands
* to show the images exist in server use below command
```
docker images
```
* to pull an image use below command
```
docker pull image_name
```
* to pull a partical version image use below command and default tag was latest
```
docker pull image_name:version or tag
```
* to create a container use below command
```
docker create image_name or image_id:version
```
* to see the running containers in docker use below command
```
docker ps
```
* to see all containers  with ids and all status use below command
```
docker ps -a
```
* to make a container start and running use below command
```
docker start container_id 
```
*  to make a container stop use below command
```
docker stop container_id 
```
* to remove a container in docker use below command running container cannot be remove first stop the container then remove
```
docker rm container_id 
docker rm -f container_id (to remove without stoping force remove)
```
* to do **PULL+CREATE+START** pull images and create container and start container at a time use below command
```
docker run image_name (it will run in foreground)

docker run -d image_name (it is for detach it will run in background)
```
* to remove images from docker use below command
```
docker rmi image_id
```
* to remove all images and containers at a time use below command
```
docker rmi `docker-images -a -q` (for images)
docker rm -f `docker ps -a -q    (for container)
```
* to see all image ids in docker use below command
```
docker images -a -q (for images)
docker ps -a -q (for container)
```
* to mentiom the port number in docker use below command
```
docker run -d -p host port-number:container port-number image_name
```
* host port-number is user defind we can give whatever we want
* container port-number is docker port it is fixed we can give our own 
* **example**
* docker run -d -p 8080:80 nginx
* alocated port cannot be used again 
* to give particular name for container in docker use below command
```
docker run -d -p host port-number:container port-number --name <write_name> image_name 
```
* duplicate names are not allowed one name is used only onetime all names must be uniqe
* to get out from the container use below command
```
ctrl + c
```
* **CONTAINER**
* container is nothing but a VM like light weight VM 
* when you run the docker image you will get the container
* container is the running version of docker image
* to login to the existing container or running container use below command
```
docker exec -it <container_id> or <name> bash
```
* now you can login with root axis and conatainer_id as there (i.e root@a764b7f56dbb:/#)
* where
* i -input & t - terminal
* after login to existing container you see as below
```
root@container_id:/#
```
* **example**
* root@a764b7f56dbb:/#
* to see the os of the container use below command
```
cat /etc/*release
```
* if we want to check the logs in docker use below command
```
docker logs <container-id>
```
* to inspect in docker container or image use below command
```
docker ps
docker inspect <container_id> or <container_name>
docker inspect <image_id> or <image_name>
```

* **NOTE**
* docker container will have **0-65535** ports
* container ports are different and vm ports are different 
* we will store multiple websites in single server and manage it with port numbers

#### Dockerfiles
* A decleretive way of createing our own image...
* We can build the docker images and push to any docker repository
* to build docker image use below command
```
docker build -t <URL>/<USER_NAME>/<IMAGE>:<VERSION> .
```
* we can buid to local without pushing by using below command
```
docker build -t <IMAGE>:<VERSION> .
```
* if we want to push what we build localy by using below command
```
docker tag -t <IMAGE>:<VERSION> .
```
* WHERE
    * URL - it is used to push docker to  docker.io(dockerhub), ECR(Elastic Container Registroy), Nexus docker registroy etc.. 
        * if we dont give any url by default it wil push to docker hub
    * -t - taging
    * dot(.) - current directory
##### Instruction
1. FROM
    * it should be the first instruction in Dockerfile
    * it is used to select base os
* **SYNTAX**
```
FROM <BASE_OS-IMAGE>:<VERSION> 
```
* **example**
```
FROM centos:8
```
* **BASEOS**
    * which os you want ex- centos, ubuntu, etc...
* **VERSION**
    * If we won't give any version by default it takes latest
2. RUN
    * It is used to install packages or configure the image inside the base OS
    * RUN instruction runs at the time of image building
* **SYNTAX**
```
RUN <PACKAGE_NAME>
```
* **example**
```
#almalinux is same as centos
FROM almalinux:8
RUN yum install nginx -y
```
3. CMD
    * CMD Instruction makes your container runing...
    * CMD runs at the time of container creation...
* **NOTE**
    * systemctl commands will not work in containers...
    * command should be foregrounded and attached to the screen then you send it into background (to do this we use -d)
* **SYNTAX**
```
CMD ["nginx","-g","daemon off;"]
```
* every word is given saparetly in double quotes "word"
4. LABEL 
    * It is used for filtering the data
* **SYNTAX**
```
LABEL key1="value"/
      key2="value"/
      key3="value"
```
* by using label we can filter the data in docker by using below command 
```
docker images --filter label=key="value"
```
* if we don't know we can use help as below
```
docker images --help
```
