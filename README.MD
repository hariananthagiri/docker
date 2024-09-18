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

### Docker installation
    * 



