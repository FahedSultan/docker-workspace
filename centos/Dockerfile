FROM centos:6

MAINTAINER Sudheer Veeravalli, https://github.com/veersudhir83

# For Administrative tasks to upgrade and install openssh-server and httpd (apache)
RUN yum -y update && yum -y upgrade && yum install -y openssh-server httpd

# Expose ports 22 and 80 to the host machine running the docker
EXPOSE 22 80