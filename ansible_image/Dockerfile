FROM ubuntu

USER root

RUN \
  apt-get update && \
  apt-get install -y software-properties-common && \
  apt-add-repository ppa:ansible/ansible && \
  apt-get update && \
  apt-get install -y --force-yes ansible

RUN \
  mv /etc/ansible/hosts /etc/ansible/hosts_bkp && \
  mkdir /ansible

# COPY ansible.cfg /etc/ansible/ansible.cfg
COPY hosts /etc/ansible/hosts

WORKDIR /ansible
