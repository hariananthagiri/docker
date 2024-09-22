# DOCKER
* docker is a containerization platform for packaging, deploying, and running applications as **CONTAINERS**
* It is a tool that create containers
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
    * process of packing applications along with its dependencies
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
#### Docker image
* it is a read only template, a lightweight standalone executable package of softwere that includes everything needed to run an application
* it contains set of instructions for createing a container that run on docker platform
* immutable
* number of layers
* Doker image = **application code + Dependencys + Libraries & Tools + Base OS**
#### Container
* a way to package applications with all necessary dependencies and configuration
* it can be easly shared
* it is like vircual meachine it does not have any OS use OS of the host
* container is nothing but a VM like light weight VM 
* when you run the docker image you will get the container
* container is the running version of docker image
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
docker run image_name:version (it will run in foreground (it show output directly))

docker run -d image_name:version (it is for detach it will run in background)
```
* to remove images from docker use below command
```
docker rmi image_id
```
* to remove all images and containers at a time use below command
```
docker rmi `docker images -a -q` (for images)
docker rm -f `docker ps -a -q    (for container)
```
* to see all image ids in docker use below command
```
docker images -a -q (for images)
docker ps -a -q (for container)
```
* to mentiom the port number in docker use below command
```
docker run -d -p host port-number:container port-number image_name:version
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
* to login to the existing container or running container use below command
```
docker exec -it <container_id> or <name>:<version> bash or sh
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
* A decleretive way of createing our own custamized image...
* We can build the docker images and push to any docker repository
* we can buid docker image localy use below command
```
docker build  <IMAGE>:<VERSION> . 
docker build -t <IMAGE>:<VERSION> . 
```
* if we want to push what we build localy by using below command
```
docker tag  <IMAGE>:<VERSION> <URL>/<USER_NAME>/<IMAGE>:<VERSION>
```
* to build docker image to docker use below command
```
docker build -t <URL>/<USER_NAME>/<IMAGE>:<VERSION> .
```
* if we want to push to docker hub what we build by using below command
```
docker tag <IMAGE>:<VERSION> <URL>/<USER_NAME>/<IMAGE>:<VERSION>
docker login
docker push <IMAGE>:<VERSION> <URL>/<USER_NAME>/<IMAGE>:<VERSION>
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
FROM <BASE-IMAGE_OS>:<VERSION> 
```
* **example**
```
FROM centos:8
```
* **BASE_IMAGE_OS**
    * which IMAGE OS you want ex- centos, ubuntu, alpine, almalinux etc...
* **VERSION**
    * If we won't give any version by default it takes latest

2. RUN
    * It is used to install packages or configure the image inside the base OS
    * RUN instruction runs at the time of image building
    * if you want to install any sotweres run any commnd or configure anything at the time of image building we use it
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
    * CMD command can be overridden by another command at run time.
        * ping google.com ping yahoo.com
* **NOTE**
    * systemctl commands will not work in containers we will manage it with CMD instruction
    * command should be foregrounded and attached to the screen then you send it into background (to do this we use -d)
* **SYNTAX**
```
CMD ["nginx","-g","daemon off;"]
```
* every word is given saparetly in double quotes and saperated with cama(,)
* we can override the given command ad run new command at run time as below
```
docker run image_name:version <command>
```

* **RUN vs CMD**
    * RUN instruction runs at the time of image building
    * CMD runs at the time of container creation
    * CMD command can be overridden by another command at run time.


4. LABEL 
    * It is used for filtering the data and images available
    * LABEL instruction add meta data to the image
* **SYNTAX**
```
LABEL key="value"
```
```
LABEL key1="value"\
      key2="value"\
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

5. EXPOSE
    * It is used to inform the docker that container runs on specific port number
    * it just gives the information related to port number and portocal
* **SYNTAX**
```
EXPOSE port-number/protocal
```
* by default it takes tcp protocal if we want we can give udp protocal
* **EXAMPLE**
```
EXPOSE 80/tcp
```

6. ENV
* it is used to pass ENV variables to docker
* **SYNTAX**
```
ENV key="value"
```
```
ENV key1="value"\
    key2="value"\
    key3="value"
```
* to see what env variables are there use below command
```
docker exec -it 3f03f9f304b1 bash
env
```
* use env command to see all env variables stored

7. COPY
    * it is used to copy file from local to image
* **SYNTAX**
```
COPY source_file destination/where/to/copy
```
* **EXAMPLE**
```
COPY index.html /usr/share/nginx/html/
``` 

8. ADD
* **COPY vs ADD**
    * both are used to copy the files from local to image... but ADD have 2 extra capabilities
        1. It can directly download files from internet
        2. It can directly untar the tar files..
* **SYNTAX**
```
ADD file.tar /desination/path/
ADD <URL> /desination/path/
```
* tar is like a ZIP file 
* **NOTE**
    * when ever you do changes rebuild the image that time

9. ENTRYPOINT
    * CMD & ENTRYPOINT both will run at container creation time
* **CMD vs ENTRYPOINT**
    1. CMD command can be overridden by another command at run time.
        * ping google.com ping yahoo.com
    2. ENTRYPOINT command can't be overridden, if you try to do so, the command you are entering at run time will go and append to ENTRYPOINT
* We can use CMD and ENTRYPOINT for best results
    1. You can use CMD instruction to supply default arguements to ENTRYPOINT
    2. user can always override CMD arguements from run time

10. USER
    * The USER instruction in a Dockerfile is a fundamental tool that determines which user will execute commands both during the image build process and when running the    container. 
    * By default, if no USER is specified, Docker will run commands as the root user, which can pose significant security risks.
* **SYNTAX**
```
USER <user_name>
```

11. WORKDIR
    * It is used to selecting the directory where we want to work
    * the instructions which we given after workdir will run in that selected directory
* **SYNTAX**
```
WORKDIR /path/of/directory
```
```
docker run -d -it work:v1
docker exec -it c02a912a bash
```

12. ARG
    * ARG variables only accessble at build time
* **ARG vs ENV**
    * ENV variables accessble at build time and run time
    * ARG instruction can provide values to the Dockerfile only at buildtime. ENV variables have access in the container
    * How can I use ARG variables inside container?
    * Assign the values of ARG to ENV variables so that you can access them inside container as well
    * ARG instruction can be used before FROM instruction to provide values to FROM instruction.
    * ARG value deffnied before FROM will not be accessed after FROM
* **SYNTAX**
```
ARG <variable_name> 
```
* if we forgot to give the variable value at build time we can give the default value as below
```
${variable_name:-default_value}
```
* variable value is passed at build time as below command
```
--build-arg variable_name=value
```
* we pass values at build time by using below command
```
docker build -t image-name:version --build-arg variable_name=value .
```
* **EXAMPLE**
```
docker build -t arg:v1 --build-arg username=hari krishna .
```
* we can use the combination of both ARG and ENV for the best result as shown below
* we can assign the arg values to the env variables so that we can access it in the container as shown below
```
ARG COURSE
ENV COURSE=${COURSE}
```

13. ONBUILD
    * author of image: I want to force users of image to follow something
    * it wont run on build time it will run at the time of other person using it
* **SYNTAX**
```
ONBUILD ADD index.html /usr/share/nginx/html/
ONBUILD RUN <command>
```

* **GENRAL SYNTAX OF DOCKER WITH INSTRUCTION**
``` Dockerfile
ARG version
FROM centos:${version:-8} # (in this way we can give default value)
RUN yum install nginx -y
CMD ["google.com"]
ENTRYPOINT ["ping"]
COPY index.html /usr/share/nginx/html/
ADD https://raw.githubusercontent.com/daws-76s/notes/master/session-01.txt /tmp/
USER $username
ARG COURSE
RUN echo "Course: ${COURSE}"
ENV COURSE=${COURSE}
LABEL TRAINING="DEVOPS"
EXPOSE 80/tcp
WORKDIR /tmp
ONBUILD ADD index.html /usr/share/nginx/html/
```
* **NOTE**
* on special exception we use ARG instruction before or prior to FROM instruction
* ARG value deffnied before FROM will not be accessed after FROM
* **SYNTAX**
``` Dockerfile
ARG version
FROM centos:8
```
* we cant use version after from instruction

* **VARIABLE**
* TO USE THE VARIABLE USE AS BELOW
```
variable_name=value
$variable_name (to use the stored variable value)
variable_name=${variable_name}
```
##### NOTE
* container cannot communicate with each other through names if they use docker default networks
##### NETWORKING
* you should not use docker default network you should create you own network
* docker contains two types of networking
    1. host
    2. bridge
* by default docker use **bridge** networking for containers
* createing own network in docker by using below command 
```
docker network (it will show multiple options)
docker network <command>
```
* it will have multiple commands as shown below
```
docker network create <network_name>
docker network connect <network_name>
docker network disconnect <network_name>
docker network rm <network_name>
docker network inspect <network_name>
docker network ls 
docker network prune (remove all unused networks)
```
* after createing network we get network id
* to see all the networks running in docker use below command
```
ifconfig
```
* to make use of our created networks while running the container use below command
```
--network=network_name
docker run -d image:version --network=network_name
```

##### DOCKER VOLUMES
* Containers are temporary when you remove the container data by default will be deleted to avide this we use volumes
* to see where the data is getteing stored use docker inspect command and goto DATA  you can see the path where it getting stored
* create a directory and give it as valume now if container is deleted even through the data is stored in that directory
* to see the valume use below command
```
find . -iname "volume.html"
```
* to save the data we use docker volumes it is of two types
1. unnamed valumes
     * it cannot be manged by docker commands
2. named volumes
    * it can be manged by docker commands
* avide running volumes with root user because it wil get full acces not recommended
* to use valumes use below command
```
-v host-path:container-path
-v /:container-path (running with root user)
```
* while createing we have to use above command as shown below
```
docker run -d -p host-port:container-port -v host-path:container-path  image:version
```
* where 
    * host-path (source-path) and valume name
    * container-path(destination-path)
* **EXAMPLE**
```
docker run -d -p 80:80 -v /home/centos/nginx-data:/usr/share/nginx/html nginx
```
* to see the commnds related to volumes use below command
```
docker volume
```
* commands related to volume as given below
```
docker volume create <volume_name>
docker volume rm <volume_name>
docker volume inspect <volume_name>
docker volume prune (to remove un used local volumes)
docker volume ls
```

##### Docker Compose
* Docker Compose is a tool for defining and running multi-container applications.
* it create network, valumes, and attach network and valumes to containers
* it Start, stop, and rebuild services
* **INSTALLATION**
* run below commands to install docker compose tool
```
sudo curl -L https://github.com/docker/compose/releases/download/1.25.4/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose 
```
* to know the commands avaialble in docker compose use below command
```
docker compose
```
* you can create a YAML file to define the services and with a single command, you can spin everything up or tear it all down.
* it contains a file with default name **docker-compose.yaml**
* **docker-compose.yaml**
* containers are nothing but services
* it is used to manage the services
```yaml
networks:
  default:
    external: true
      name: <network-name>
services:
  mongodb:
    image: mongodb:v1
    container_name: mongodb
    volumes:
    - source: mongodb
      target: /data/db
      type: volume
    ports:
    - "80:80"
    depends_on:
    - catalogue
    - cart
```
* to build all containers at a time use below command
```
docker compose up -d
```
* to remove all containers at a time use below command
```
docker compose down
```
* **NOTE**
* JAVA SPECIALITY
* Source code ==> compile ==> byte code(jar file) ==> run byte code
* JDK ==> java devlopment kit
* JRE ==> java runtime environment
* JDK > JRE JRE is a subset of JDK 
* JDK(memory) > JRE(memory)


##### BEST PRACTICES IN DOCKER
1. use official images
    * official images contains bradge
2. create custom dockerfiles
    * avide modifying official images and create your own image based on your requirement
3. keep containers small
    * use smaller images that reduces the deployment time and disk space by using bare minimum os
        * ex:- alpine, distro, coreos e.t.c
4. use multi stage builds
5. use docker valumes to protest data
6. use custom networks to isolate containers from other projects
7. remove unused data from docker by using below command
    ```
    docker system prune 
    ```
###### DOCKER ARCHITECTURE
1. docker shell/ docker command send a request to docker deamon
2. docker engine receives the request
3. it will check whether image is available in local or not
4. if available it will create container and show the response in client
5. if not available, it will pul from docker central hub, keep it in local.
6. create container and response to client

###### DOCKER LAYERS
* 