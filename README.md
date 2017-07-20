# All Docker commands listed for handy use

### Plain docker commands 
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
$ docker run â€“d â€“P friendlyname --> Map the port exposed by the container over a random available ip on the host 
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
       **Example:** docker exec <container_id>  apt-get update --> Update repository 
       **Example:** docker exec <container_id>  apt-get install ant -y --> Install ant 
       **Example:** docker exec <container_id>  apt-get install maven -y --> Install maven 

     When logged into the docker container, Type Ctrl+p, Ctrl+q will help you to turn interactive mode to daemon mode, which will keep the container running as daemon. Use 'exit' to stop the container and come out to main console. 
``` 

### General Docker Instructions
> * Docker containers, volumes etc.. will be available at **/var/lib/docker/** folder
> * To Update/Install softwares on Jenkins container: 
    Use **'-u root'** in the docker run command        # Start the docker container as root:  
> * To run docker command without sudo, you need to add your user (who has root privileges) to docker group. For this run following command: 
```sh 
$ sudo usermod -aG docker $USER
Example: sudo usermod -aG docker ubuntu/ec2-user/jenkins/<whatever user> 
```
   
### Commands to start/manage jenkins docker: 
```sh 
$ docker run -p 9999:8080 -p 50000:50000 -v /home/<user_account>/docker-jenkinsvolume/jenkins_home:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock -v $(which docker):/usr/bin/docker --name myjenkins veersudhir83/myjenkins:latest 
```
Be sure to point the -v and -p switches to the right ports, volumes etc..

### To create a stack 
```sh
$ docker stack ls --> List all running applications on this Docker host
```
```sh 
$ docker stack deploy -c <composefile> <appname> --> Run the specified Compose file
```
```sh 
$ docker stack services <appname> --> List the services associated with an app
```
```sh 
$ docker stack ps <appname> --> List the running containers associated with an app
```
```sh 
$ docker stack rm <appname> --> Tear down an application 
``` 
 
### Dealing with Proxy - (Ubuntu / Any Linux system - path of the config may change - setting will be same) 
Usually even after setting proxy in the network settings and default browser, unless registering the SSL certificate, we get the error x509 certificate signed by unknown authority.  
 
Do the following to solve this: 
> * Add proxy entries in **/etc/default/docker file**
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
  
> * If using Dockerfile - Add the following lines at the top of your Dockerfile: 
  ENV http_proxy 'http://user:password@proxy-host:proxy-port' 
  ENV https_proxy 'http://user:password@proxy-host:proxy-port' 
  ENV HTTP_PROXY 'http://user:password@proxy-host:proxy-port' 
  ENV HTTPS_PROXY 'http://user:password@proxy-host:proxy-port' 
 
> * Convert the certificate file from .cer to .crt  
```sh
$ openssl x509 -inform PEM -in PCAcert.cer -out PCAcert.crt 
``` 
> * Place the .crt file into **/usr/share/ca-certificates/mozilla**
 
> * Run the below command which will install the .pem format of the certificate into **/etc/ssl/certs**
```sh
$ dpkg-reconfigure ca-certificates 
``` 
 
########################################################## 

[//]: # (These are reference links used in the body of this note and get stripped out when the markdown processor does its job. There is no need to format nicely because it shouldn't be seen.)

   [htaccesstool]: <http://www.htaccesstools.com/htpasswd-generator/>
