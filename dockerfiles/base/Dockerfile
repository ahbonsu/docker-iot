# ------------------------------------------------------------------------
#
# Copyright 2017 WSO2, Inc. (http://wso2.com)
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License
#
# ------------------------------------------------------------------------

# set to latest Ubuntu LTS
FROM ubuntu:18.04
MAINTAINER WSO2 Docker Maintainers "dev@wso2.org"

# set user configurations
ARG USER=wso2carbon
ARG USER_ID=802
# set dependant files directory
ARG FILES=./files
# set wso2 product configurations
ARG WSO2_HOME=/wso2
ARG WSO2_SERVER=wso2iot
ARG WSO2_SERVER_VERSION=3.3.0
ARG WSO2_SERVER_DIST=${WSO2_SERVER}-${WSO2_SERVER_VERSION}
ARG WSO2_SERVER_HOME=${WSO2_HOME}/${WSO2_SERVER_DIST}
# set jdk configurations
ARG JDK_DIST=jdk1.8.0*
ARG JAVA_HOME=${WSO2_HOME}/java

# install required packages
RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends --no-install-suggests \
    curl netcat && \
    rm -rf /var/lib/apt/lists/* && \
    echo '[ ! -z "$TERM" -a -r /etc/motd ] && cat /etc/motd' \
    >> /etc/bash.bashrc \
    ; echo "\
    Welcome to WSO2 Docker resources.\n\
    The Docker container contains the WSO2 product with its latest updates, which are under the End User License Agreement (EULA) 2.0.\n\
    \n\
    Read more about EULA 2.0 (https://wso2.com/licenses/wso2-update/2.0).\n"\
    > /etc/motd

# create a user
RUN useradd --system --no-log-init -u ${USER_ID} ${USER}

# copy jdk, wso2 product distribution to WSO2 home directory and copy mysql connector jar to product distribution
COPY ${FILES}/${JDK_DIST} ${JAVA_HOME}
COPY ${FILES}/${WSO2_SERVER_DIST} ${WSO2_SERVER_HOME}
COPY ${FILES}/mysql-connector-java-*-bin.jar ${WSO2_SERVER_HOME}/lib

RUN chown -R ${USER}:0 ${WSO2_HOME} && \
    chmod -R g=u ${WSO2_HOME}

# set environment variables
ENV JAVA_HOME=${JAVA_HOME} \
    PATH=$JAVA_HOME/bin:$PATH \
    WSO2_SERVER_HOME=${WSO2_SERVER_HOME} \
    WORKING_DIRECTORY=${WSO2_HOME}
