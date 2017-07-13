# Ubuntu Latest with Java 8 installed.

# Build image with:  docker build -t krizsan/ubuntu1504java8:v1 .

FROM ubuntu:latest

MAINTAINER Ivan Krizsan, https://github.com/veersudhir83

ENV http_proxy 'http://proxy-tvm.quest-global.com:8080' 
ENV https_proxy 'http://proxy-tvm.quest-global.com:8080'

RUN apt-get update && \

    apt-get upgrade -y && \

    apt-get install -y  software-properties-common && \

    add-apt-repository ppa:webupd8team/java -y && \

    apt-get update && \

    echo oracle-java7-installer shared/accepted-oracle-license-v1-1 select true | /usr/bin/debconf-set-selections && \

    apt-get install -y oracle-java8-installer && \

    apt-get clean
