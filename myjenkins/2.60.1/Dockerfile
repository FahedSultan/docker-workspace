FROM jenkins:2.60.1

MAINTAINER Sudheer Veeravalli, https://github.com/veersudhir83

# For Administrative tasks
# USER root
# RUN apt-get update && apt-get -y upgrade && apt-get -y install --no-install-recommends jq

# If you want to get rid of admin password setup
ENV JAVA_OPTS="-Djenkins.install.runSetupWizard=false"

# If you want to run this behind a proxy
# ENV http_proxy 'http://proxy-tvm.quest-global.com:8080'
# ENV https_proxy 'http://proxy-tvm.quest-global.com:8080'
# ENV HTTP_PROXY 'http://proxy-tvm.quest-global.com:8080'
# ENV HTTPS_PROXY 'http://proxy-tvm.quest-global.com:8080'

# If you want to install any plugins automatically
# COPY plugins.txt /usr/share/jenkins/ref/plugins.txt
# RUN /usr/local/bin/install-plugins.sh < /usr/share/jenkins/ref/plugins.txt

# Drop back to the regular jenkins user - good practice
# USER jenkins

# Expose port 8080 to the host machine running the docker
EXPOSE 8080
