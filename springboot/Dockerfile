############################################################
# Dockerfile to build apache tomcat container images
# Based on official tomcat latest image
############################################################
FROM anapsix/alpine-java

# Maintainer
MAINTAINER "sudheer veeravalli <veersudhir83@gmail.com>"

# Copy war file to tomcat webapps folder
COPY devops-web.war /home/devops-web.war

CMD ["java","-jar","/home/devops-web.war"]
