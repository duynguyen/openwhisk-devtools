FROM ubuntu:trusty
ENV DEBIAN_FRONTEND noninteractive
ENV UCF_FORCE_CONFFNEW YES
RUN ucf --purge /boot/grub/menu.lst

# install openwhisk
RUN apt-get -y update && \
    apt-get -y upgrade && \
    apt-get install -y \
      git \
      curl \
      apt-transport-https \
      ca-certificates \
      python-pip \
      python-dev \
      libffi-dev \
      libssl-dev \
      libxml2-dev \
      libxslt1-dev \
      libjpeg8-dev \
      zlib1g-dev

# clone OpenWhisk and install dependencies
# Note that we are not running the install all script since we do not care about Docker.
RUN git clone https://github.com/openwhisk/openwhisk && \
    /openwhisk/tools/ubuntu-setup/misc.sh && \
    /openwhisk/tools/ubuntu-setup/pip.sh && \
    /openwhisk/tools/ubuntu-setup/java8.sh && \
    /openwhisk/tools/ubuntu-setup/scala.sh && \
    /openwhisk/tools/ubuntu-setup/ansible.sh

# Change this to https://github.com/openwhisk/openwhisk-devtools when committing to master
COPY ansible /openwhisk-devtools/kubernetes/ansible
COPY configure /openwhisk-devtools/kubernetes/configure
RUN mkdir /openwhisk-devtools/kubernetes/ansible/group_vars

# install kube dependencies
# Kubernetes assumes that the version is 1.5.0+
RUN wget https://storage.googleapis.com/kubernetes-release/release/v1.5.0/bin/linux/amd64/kubectl && \
    chmod +x kubectl && \
    mv kubectl /usr/local/bin/kubectl

# install wsk cli
RUN wget https://openwhisk.ng.bluemix.net/cli/go/download/linux/amd64/wsk && \
    chmod +x wsk && \
    mv wsk /openwhisk/bin/wsk
