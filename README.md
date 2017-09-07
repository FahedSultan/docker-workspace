# All Docker commands listed for handy use

## Plain docker commands 
```sh 
$ docker info --> Show info about the docker installed on the host machine 
```
```sh 
$ docker build -t friendlyname . --> Create image using this directory's Dockerfile 
```
```sh 
$ docker run -p 4000:80 friendlyname --> Run "friendlyname" mapping port 4000 to 80 
```
```sh 
$ docker run -d -p 4000:80 friendlyname --> Same thing, but in detached mode 
```
```sh 
$ docker run -d -p friendlyname --> Map the port exposed by the container over a random available ip on the host 
```
```sh 
$ docker ps  --> See a list of all running containers 
```
```sh 
$ docker stop <hash> --> Gracefully stop the specified container 
```
```sh 
$ docker ps -a --> See a list of all containers, even the ones not running 
```
```sh 
$ docker kill <hash> --> Force shutdown of the specified container 
```
```sh 
$ docker rm <hash> --> Remove the specified container from this machine 
```
```sh 
$ docker rm $(docker ps -a -q) --> Remove all containers from this machine 
```
```sh 
$ docker rm $(docker ps -a | awk '{print $NF}' | grep -w <name>) --> Remove all containers from this machine matching the given name 
```
```sh 
$ docker images -a --> Show all images on this machine 
```
```sh 
$ docker rmi <imagename> --> Remove the specified image from this machine 
```
```sh 
$ docker rmi $(docker images -q) --> Remove all images from this machine 
```
```sh 
$ docker login --> Log in this CLI session using your Docker credentials 
``` 
```sh 
$ docker commit <container_id>  <image_id> --> commit the container state to image 
```
```sh 
$ docker tag <image> username/repository:tag --> Tag <image> for upload to registry
``` 
```sh 
$ docker push username/repository:tag --> Upload tagged image to registry
``` 
```sh 
$ docker run username/repository:tag --> Run image from a registry
``` 
```sh 
$ docker start -it <container-name> /bin/bash --> Opens a tty in interactive mode
``` 
```sh 
$ docker start $(docker ps -a | awk '{print $NF}' | grep -w devops-web) --> Starts all containers matching the given name
``` 
```sh 
$ docker start -p -d <container-name> --> Runs the container in daemon mode
``` 
```sh 
$ docker run -i -t -e TZ=IST -p 9991:8080 --name tommy tommy:latest --> Runs the docker with specific time zone so the logs will seem relevant or                                                                                                                                                    else it uses only UTC
``` 
```sh 
$ docker inspect --format='{{(index (index .NetworkSettings.Ports "8080/tcp") 0).HostPort}}' devops-web-tommy8.5-2 --> To fetch Host's port number
``` 
```sh 
$ docker inspect --format='{{json .State}}' <Container_name/id> --> To get the content of a specific settings in the docker container
``` 
```sh 
$ docker cp /root/file.txt docker-container:/root --> To copy a file from host to the docker container  
       **Example:** docker cp /home/edureka/devops/repos/docker-workspace/tommy/devops-web.war tommy:/usr/local/tomcat/webapps/
```  
```sh 
$ docker exec -it <container-name/id> /bin/bash --> Login to bash on the container file system 
``` 
     **Example:** docker exec <container_id>  apt-get update --> Update repository 
     **Example:** docker exec <container_id>  apt-get install ant -y --> Install ant 
     **Example:** docker exec <container_id>  apt-get install maven -y --> Install maven 

     When logged into the docker container, Type **Ctrl+p**, **Ctrl+q** will help you to turn interactive mode to daemon mode, which will keep the container running as daemon. Use 'exit' to stop the container and come out to main console. 


## General Docker Instructions
> * Docker containers, volumes etc.. will be available at **/var/lib/docker/** folder
> * To Update/Install softwares on Jenkins container: 
    Use **'-u root'** in the docker run command        # Start the docker container as root:  
> * To run docker command without sudo, you need to add your user (who has root privileges) to docker group. For this run following command: 
```sh 
$ sudo usermod -aG docker $USER
Example: sudo usermod -aG docker ubuntu/ec2-user/jenkins/<whatever user> 
```
> Along with above **usermod** command, you may also have to give permission to the **/var/run/docker.sock** file on the host running the docker using
```sh
$ chmod 777 /var/run/docker.sock
```
Note: Instead of **777**, **ugo+x** may also be used in the above command to be more specific

   
## Commands to start/manage jenkins docker: 
```sh 
$ docker run -p 8080:8080 -p 50000:50000 -v /home/<user_account>/docker-jenkinsvolume/jenkins_home:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock -v $(which docker):/usr/bin/docker --name myjenkins veersudhir83/myjenkins:latest 
```
**Important Note:** Be sure to point the -v and -p switches to the right ports, volumes etc..

## Using Docker Swarm to deploy an application 
- Reference Instructions at [swarmreference]
### Set up a Docker registry
> Start the registry as a service on your swarm:
```sh
$ docker service create --name <name_of_registry> --publish 5000:5000 registry:2
```
Once it reads 1/1 under REPLICAS, it’s running. If it reads 0/1, it’s probably still pulling the image; wait a little bit for it to pull the image.

> Check the status of registry service
```sh
$ docker service ls
```
> Bring the registry down with
```sh
$ docker service rm <name_of_registry>
```

### Create an application using a program, a Dockerfile, and docker-compose.yml
Refer to the sample code here [stackdemo]
> Test the app with Compose
```sh
$ docker-compose up -d
```
> Check that the app is running using below command
```sh
$ docker-compose ps
```
You can test the app with curl: 
```sh
$ curl http://localhost:8000
Hello World! I have been seen 1 times.
```
> * Once verified that app is up and running, you can bring this app down as this is not the way you want. You ultimately want to deploy this as a stack onto a docker swarm (with help of registry) which acts as a load balancer and help scaling up as per your needs.
> * To bring down the app
```sh
$ docker-compose down --volumes
```
> Push the generated image to the registry
```sh
docker-compose push
```
> Once the stack is ready, we will now deploy the stack to the swarm using below steps

### Create a stack 
> List all stacks on the host
```sh
$ docker stack ls 
```
> Deploy the stack using the compose file (Note: compose file may be using the Dockerfile in its structure which will create the necessary containers)
```sh 
$ docker stack deploy -c <composefile> <stack_name> 
```
> List the services associated with the stack
```sh 
$ docker stack services <stack_name> 
```
You can test the app with curl: 
```sh
$ curl http://localhost:8000
Hello World! I have been seen 1 times.
```
> List the services associated with the stack
```sh 
$ docker stack ps <appname>
```
> Bring the stack down using
```sh 
$ docker stack rm <stack_name>
``` 
 
## Dealing with Proxy - (Ubuntu / Any Linux system - path of the config may change - setting will be same) 
Usually even after setting proxy in the network settings and default browser, unless registering the SSL certificate, we get the error x509 certificate signed by unknown authority.  
 
Do the following to solve this: 
> Add proxy entries in **/etc/default/docker file**
```sh
$ [2017-06-29 09:32:41] root@test01  /home/edureka $ cat /etc/default/docker  
  #Docker Upstart and SysVinit configuration file 
  #THIS FILE DOES NOT APPLY TO SYSTEMD 
  #Please see the documentation for "systemd drop-ins": 
  #https://docs.docker.com/engine/articles/systemd/ 
  #Customize location of Docker binary (especially for development testing). 
  #DOCKERD="/usr/local/bin/dockerd" 
  #Use DOCKER_OPTS to modify the daemon startup options. 
  #DOCKER_OPTS="--dns 8.8.8.8 --dns 8.8.4.4" 
 
  #If you need Docker to use an HTTP proxy, it can also be specified here. 
  #export http_proxy="http://127.0.0.1:3128/" 
  export http_proxy="http://proxy-tvm.quest-global.com:8080" 
  export https_proxy="http://proxy-tvm.quest-global.com:8080" 
```
  
> If using Dockerfile - Add the following lines at the top of your Dockerfile: 
```sh
  ENV http_proxy 'http://user:password@proxy-host:proxy-port' 
  ENV https_proxy 'http://user:password@proxy-host:proxy-port' 
  ENV HTTP_PROXY 'http://user:password@proxy-host:proxy-port' 
  ENV HTTPS_PROXY 'http://user:password@proxy-host:proxy-port' 
``` 
> Convert the certificate file from .cer to .crt  
```sh
$ openssl x509 -inform PEM -in PCAcert.cer -out PCAcert.crt 
``` 
> Place the .crt file into **/usr/share/ca-certificates/mozilla**
 
> Run the below command which will install the .pem format of the certificate into **/etc/ssl/certs**
```sh
$ dpkg-reconfigure ca-certificates 
``` 
# Solutions to some problems using Docker-inside-Docker (Dind)
> When you face an issue using accessing host docker instance within the jenkins (running as a docker container on the same host) as below 
    docker: error while loading shared libraries: libltdl.so.7: cannot open shared object file: No such file or directory
Solution:
1. Run the below command on both host and the docker container
```sh
$ ldd $(which docker)
```
In the Jenkins (inside docker), You may get an output as below saying some of the linked libraries not found
```sh
        linux-vdso.so.1 (0x00007ffd27160000)
        libpthread.so.0 => /lib/x86_64-linux-gnu/libpthread.so.0 (0x00007fa89f9e0000)
        libltdl.so.7 => not found
        libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007fa89f641000)
        /lib64/ld-linux-x86-64.so.2 (0x000055cdefdb1000)
```
Where as the same command on the host running docker which not yield any errors and shows the necessary file. 
```sh
        linux-vdso.so.1 =>  (0x00007ffcb8127000)
        libpthread.so.0 => /lib/x86_64-linux-gnu/libpthread.so.0 (0x00007f788344d000)
        libltdl.so.7 => /usr/lib/x86_64-linux-gnu/libltdl.so.7 (0x00007f7883243000)
        libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f7882e78000)
        /lib64/ld-linux-x86-64.so.2 (0x0000560e85dcb000)
        libdl.so.2 => /lib/x86_64-linux-gnu/libdl.so.2 (0x00007f7882c74000)
```
2. Just copy it from host to the container along with symlink and physical files
Example
```sh
$ docker cp  /usr/lib/x86_64-linux-gnu/libltdl.so.7.3.1 myjenkins:/lib/x86_64-linux-gnu/libltdl.so.7.3.1
$ docker cp  /usr/lib/x86_64-linux-gnu/libltdl.so.7 myjenkins:/lib/x86_64-linux-gnu/libltdl.so.7
```


## Maintainers
> * [Sudheer Veeravalli](https://github.com/veersudhir83)

[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen.)

   [stackdemo]: <https://github.com/veersudhir83/docker-workspace/tree/master/stackdemo/>
   [swarmreference]: <https://docs.docker.com/engine/swarm/stack-deploy/#deploy-the-stack-to-the-swarm/>
