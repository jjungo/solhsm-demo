FROM img-base

MAINTAINER Joel Jungo <j.jungo@gmail.com>

RUN     apt-get update && apt-get upgrade -y \
        && apt-get install stunnel apache2 libssl-dev -y

WORKDIR /root/install/

RUN     git clone https://github.com/jjungo/solhsm-engine.git 

WORKDIR /root/install/solhsm-engine
    
ADD     ./cert/demo.org/demo.crt /cert/
RUN     sed -r -i 's,engineCtrl=ZMQ_SERVER_PUB_CERT_PATH:/data/keys/server.cert,engineCtrl=ZMQ_SERVER_PUB_CERT_PATH:/data/engine/keys/server.cert,g' apache.conf \
        && sed -r -i 's,engineCtrl=ZMQ_CLIENT_PRIV_CERT_PATH:/data/keys/priv/client.cert_secret,engineCtrl=ZMQ_CLIENT_PRIV_CERT_PATH:/data/engine/keys/priv/client.cert_secret,g' apache.conf \
        && sed -r -i 's,cert = /data/cert/your_tls_cert.crt,cert = /cert/demo.crt,g' apache.conf \
        && sed -r -i 's,engineCtrl=ZMQ_SERVER_IP:127.0.0.1,engineCtrl=ZMQ_SERVER_IP:<host_ip>,g' apache.conf

RUN     make && make install

RUN     cp ./apache.conf /etc/stunnel/stunnel.conf \
        && cp ./run.sh /usr/bin/run

RUN     chmod +x /usr/bin/run \
        && echo  " \
<h2>Welcome to solhsm web server</h2> \
<p>This is only a test page.</p> \
"        > /var/www/html/index.html

EXPOSE  443 80

WORKDIR /
RUN     echo "DO NOT forget to share your public key with your HSM"

CMD ["/usr/bin/run"]



