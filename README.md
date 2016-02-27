# solhsm-demo
[Simple Open & Light HSM](https://github.com/jjungo/solhsm-core) project  allows
OpenSSL's users to enhance security by storing private keys on a Harware
Security Module based on a BeagleBoneBlack (or whatever if you want). The
Hardware Security Module works with the solhsm-PROTOCOL in order to communicate
over IP with an OpenSSL ENGINE ([solhsm-engine](https://github.com/jjungo/solhsm-engine)).

This repository is a full working demo example that provide two Docker
containers:

* web-server: Contains SSL/TLS proxy over Stunnel, http Apache server, OpenSSL
with our ENGINE.
* solhsm: Contains the [hsm-core](https://github.com/jjungo/solhsm-core)
application and the [solhsm-mgmt](https://github.com/jjungo/solhsm-mgmt) tool.

The TLS certificate oon the server side is self signed, the private key is of
course already set into database on the hsm side.

## Build
Change the host ip into the `engine/Dockerfile` before build images.

Build these three Docker images:

    host-machine:$ docker build -t img-base img-base/.
    host-machine:$ docker build -t solhsm hsm-core-mgmt/.
    host-machine:$ docker build -t web-server engine/.

## Run
Run the hsm core and the web server with engine

    host-machine:$ docker run -it -p 9222:9222 -v $(pwd)/data:/data solhsm
    root@HSM-CONTAINER:# /etc/init.d/hsm-core start
    host-machine:$ docker run -p 80:80 -p 443:443 -it -v $(pwd)/data:/data web-server

## Test
On a host machine you can access to the web server

    host-machine:$ curl https://localhost --insecure

`--insecure` parameter is to avoid CA verification, our demo TLS certificate is self signed.
