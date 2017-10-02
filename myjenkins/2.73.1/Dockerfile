FROM jenkins:2.73.1

MAINTAINER Sudheer Veeravalli, https://github.com/veersudhir83

# If we want to install via apt
# RUN apt-get update && apt-get install -y ruby make more-things-here
USER root
RUN apt-get update && apt-get -y install jq

# If you want to get rid of admin password setup
# ENV JAVA_OPTS="-Djenkins.install.runSetupWizard=false"

# If you want to run this behind a proxy
# ENV http_proxy 'http://proxy-host:proxy-port:8080'
# ENV https_proxy 'http://proxy-host:proxy-port:8080'

COPY plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN /usr/local/bin/install-plugins.sh < /usr/share/jenkins/ref/plugins.txt

# Drop back to the regular jenkins user - good practice
USER jenkins

# Expose port 8080 to the host machine running the docker
EXPOSE 8080
