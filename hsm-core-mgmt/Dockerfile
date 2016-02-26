FROM img-base

MAINTAINER Joel Jungo <j.jungo@gmail.com>

############################## solhsm core ###################################
WORKDIR /root/install
RUN     git clone https://github.com/jjungo/solhsm-core.git /root/install/solhsm-core

WORKDIR /root/install/solhsm-core/

RUN     sed -r -i 's,char \*server_cert_file = "/etc/hsm/server/server.cert_secret";,char *server_cert_file = "/data/hsm/keys/priv/server.cert_secret";,g' src/solhsm-core.c \
        && sed -r -i 's,char \*client_cert_path = "/etc/hsm/server/pub_key";,char *client_cert_path = "/data/hsm/keys/";,g' src/solhsm-core.c

## core installation
RUN     chmod +x autogen.sh
RUN     ./autogen.sh   \
        && ./configure \
        && make \
        && make install \
        && make cleanall

############################## solhsm mgmt ###################################

RUN     pip3 install pycrypto
WORKDIR /root/install

## need to be pull from git repo
RUN     git clone https://github.com/jjungo/solhsm-mgmt.git
WORKDIR /root/install/solhsm-mgmt

## mgmt installation
RUN     cd ./lib/ \
        && python3 setup.py install
RUN     cp solhsm-mgmt.py /usr/bin/ \
        && chmod +x /usr/bin/solhsm-mgmt.py

WORKDIR /home/hsmadmin
