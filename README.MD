# DOCKER
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
* **container is the running version of image**
* we will get all these images pulled from docker hub we have to signup to docker hub account  

### Docker installation
* create a server(centos) by loging in to aws account 
* go to chrome type docker install click on docker engine select centos

* **SYNTAX**
```
sudo yum install -y yum-utils
sudo yum-config-manger --add-repo https://download.docker.com/linux/centos/docker-ce.repo
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
* to pull a partical version image use below command
```
docker pull image_name:version
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
* to do **PULL+CREATE+START** images at a time use below command
```
docker run image_name (it will run in foreground)

docker run -d image_name (it is for detach it will run in background)
```
* to remove images from docker use below command
```
docker rmi image_id
```
* to remove all images at a time use below command
```
docker rmi 'docker-images -a -q'
```
* to see only existing image ids in docker use below command
```
docker images -a -q
```
* to mentiom the port number in docker use below command
```
docker run -d -p host port-number:container port-number image_name
```
* **example**
* docker run -d -p 8080:80 nginx
* alocated port cannot be used again 
* to give particular name for container in docker use below command
```
docker run -d -p host port-number:container port-number --name <write_name> image_name 
```
* **NOTE**
* docker container will have **0-65535** ports
* container ports are different and vm ports are different 
* we will store multiple websites in single server and manage it with port numbers



