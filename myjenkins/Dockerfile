FROM jenkins:2.60.1

MAINTAINER Sudheer Veeravalli, https://github.com/veersudhir83

# If we want to install via apt
# RUN apt-get update && apt-get install -y ruby make more-things-here
USER root
RUN apt-get update && apt-get -y install jq

# If you want to get rid of admin password setup
# ENV JAVA_OPTS="-Djenkins.install.runSetupWizard=false"

# If you want to run this behind a proxy
# ENV http_proxy 'http://proxy-tvm.quest-global.com:8080'
# ENV https_proxy 'http://proxy-tvm.quest-global.com:8080'

# Install necessary plugins
#  Capture plugins list using
# $ With Versions:
#   curl -sSL "http://<jenkins_user_name>:<jenkins_password>@<jenkins_url>:8080/pluginManager/api/xml?depth=1&xpath=/*/*/shortName|/*/*/version&wrapper=plugins" | perl -pe 's/.*?<shortName>([\w-]+).*?<version>([^<]+)()(<\/\w+>)+/\1 \2\n/g'|sed 's/ /:/'

# $ Without Versions:
#   1) curl "http://localhost:8080/pluginManager/api/json?depth=1" | jq -r '.plugins[].shortName'
#   2) docker exec -it myjenkins ls /var/jenkins_home/plugins/ | grep -v jpi
#   3) Using CLI: java -jar jenkins-cli.jar -s http://localhost:8080/ list-plugins

COPY plugins.txt /usr/share/jenkins/ref/plugins.txt
RUN /usr/local/bin/install-plugins.sh < /usr/share/jenkins/ref/plugins.txt

# Drop back to the regular jenkins user - good practice
USER jenkins

# Expose port 8080 to the host machine running the docker
EXPOSE 8080
