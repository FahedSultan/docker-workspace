############################################################
# Dockerfile to build apache tomcat container images
# Based on official tomcat latest image
############################################################
FROM tomcat:8.5-jre8

# Maintainer
MAINTAINER "sudheer veeravalli <veersudhir83@gmail.com>"

# Copy war file to tomcat webapps folder 
COPY devops-web.war /usr/local/tomcat/webapps/

# Make port 80 available to the world outside this container
EXPOSE 8080
