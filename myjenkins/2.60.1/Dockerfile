FROM jenkins:2.60.1

MAINTAINER Sudheer Veeravalli, https://github.com/veersudhir83

# If we want to install via apt
USER root

ENV GOSU_VERSION 1.9
RUN set -x \
    && apt-get update && apt-get install -y --no-install-recommends ca-certificates wget && rm -rf /var/lib/apt/lists/* \
    && dpkgArch="$(dpkg --print-architecture | awk -F- '{ print $NF }')" \
    && wget -O /usr/local/bin/gosu "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$dpkgArch" \
    && wget -O /usr/local/bin/gosu.asc "https://github.com/tianon/gosu/releases/download/$GOSU_VERSION/gosu-$dpkgArch.asc" \
    && export GNUPGHOME="$(mktemp -d)" \
    && gpg --keyserver ha.pool.sks-keyservers.net --recv-keys B42F6819007F00F88E364FD4036A9C25BF357DD4 \
    && gpg --batch --verify /usr/local/bin/gosu.asc /usr/local/bin/gosu \
    && rm -r "$GNUPGHOME" /usr/local/bin/gosu.asc \
    && chmod +x /usr/local/bin/gosu \
    && gosu nobody true


# RUN apt-get update && apt-get -y install jq
COPY entrypoint.sh /entrypoint.sh
RUN chmod u+x /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
CMD ["jenkins", "/bin/tini", "--", "/usr/local/bin/jenkins.sh"]


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
USER jenkins

# Expose port 8080 to the host machine running the docker
EXPOSE 8080
