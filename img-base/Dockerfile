FROM ubuntu:14.04.3

MAINTAINER Joel Jungo <j.jungo@gmail.com>

RUN     apt-get update && apt-get upgrade -y \
        && apt-get install -y \
        vim \
        gcc \
        g++ \
        git \
        libpgm-dev \
        cmake \
        automake \
        libtool \
        pkg-config \
        libsqlite3-dev \
        sqlite3 \
        rsyslog \
        python3 \
        python3-pip \
        libssl-dev

WORKDIR /root/install

## Install Dependencies and compile them (libsodium, libzmq, czmq)

## need to be pull from git repo
COPY ./dep.sh /root/install/
RUN     /bin/bash dep.sh

# Set locale time and timezone
RUN     cp /usr/share/zoneinfo/Europe/Zurich /etc/localtime \
        && echo "Europe/Zurich" > /etc/timezone
