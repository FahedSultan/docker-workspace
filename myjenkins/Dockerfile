FROM jenkins:2.60.1

MAINTAINER Sudheer Veeravalli, https://github.com/veersudhir83

# if we want to install via apt
# RUN apt-get update && apt-get install -y ruby make more-thing-here
USER root 

# ENV http_proxy 'http://proxy-tvm.quest-global.com:8080' 
# ENV https_proxy 'http://proxy-tvm.quest-global.com:8080'

COPY plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN /usr/local/bin/install-plugins.sh < /usr/share/jenkins/ref/plugins.txt

# drop back to the regular jenkins user - good practice
USER jenkins 

EXPOSE 8080
