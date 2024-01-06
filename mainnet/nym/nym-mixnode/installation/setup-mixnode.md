---
description: Installation and Setup NYM Mixnode
---

# Setup Mixnode

{% hint style="info" %}
Running As User : _salinem_
{% endhint %}

Create file Dockerfile-Mixnode in /mainnet/salinem/

```bash
FROM bitnami/minideb:buster AS builder

RUN set -ex \
    && install_packages libssl1.1 ca-certificates curl python3 python3-pip \
    && useradd -U -m -s /sbin/nologin nym

WORKDIR /home/nym

COPY bin/mixnode.sh .
COPY .env_mixnode .
COPY bin/nym-mixnode .

# Change onwership and permissions
RUN chmod +x mixnode.sh && chown -R nym:nym ./
RUN pip3 install requests
RUN chmod +x nym-mixnode

USER nym

VOLUME [ "/home/nym/.nym" ]

EXPOSE 1789

CMD [ "./mixnode.sh" ]


```

And create file run.sh

```bash
#!/bin/bash
#
# RoomIT
# https://roomit.xyz
# If this script useful and you will visit cikarang indonesia,
# Let's drink coffee and talk about blockchain
#
# Script Automatic Build and Running Container NYM
# admin@roomit.xyz
# 
#

# Container Mode Host
OPTIONS="$1"
SERVICE="$2"
STORAGE="/mainnet/salinem/data"


function help(){
   echo "bash run.sh [ build | container | full-update ]   [ mixnode | client | requester ]"
}

function service(){
mkdir -p ${STORAGE}
if [ "${SERVICE}" == "mixnode" ]
then
    docker run -d --name nym-mixnode-server --cap-add=NET_ADMIN --cap-add=NET_RAW --network host  -v ${STORAGE}:/home/nym/.nym nym-mixnode-server/latest
    echo mixnode run container
elif [ "${SERVICE}" == "client" ]
then
    docker run -d --name nym-client-server --cap-add=NET_ADMIN --cap-add=NET_RAW --network host  -v ${STORAGE}:/home/nym/.nym nym-client-server/latest
    echo client run container
elif [ "${SERVICE}" == "requester" ]
then
    docker run -d --name nym-requester-server --cap-add=NET_ADMIN --cap-add=NET_RAW --network host  -v ${STORAGE}:/home/nym/.nym nym-requester-server/latest
    echo requester run container
else
   help;
fi
}

function build(){
if [ "${SERVICE}" == "mixnode" ]
then
    echo mixnode run build
    docker build -f Dockerfile-Mixnode -t nym-mixnode-server/latest .
elif [ "${SERVICE}" == "client" ]
then
    echo client run build
    docker build -f Dockerfile-Client -t nym-client-server/latest .
elif [ "${SERVICE}" == "requester" ]
then
    echo requester run build
    docker build -f Dockerfile-Requester -t nym-requester-server/latest .
else
   help;
fi
}


function full:update(){
build;
mkdir -p ${STORAGE}
if [ "${SERVICE}" == "mixnode" ]
then
    check_container=`docker ps -a | grep  "\bnym-${SERVICE}-server\b" | grep -v "\bnym-${SERVICE}-server\b"`
    docker stop nym-${SERVICE}-server nym-${SERVICE}-server-old 2> /dev/null
    docker rm nym-${SERVICE}-server-old
    docker rename nym-${SERVICE}-server nym-${SERVICE}-server-old
    docker run -d --name nym-mixnode-server --cap-add=NET_ADMIN --cap-add=NET_RAW --network host  -v ${STORAGE}:/home/nym/.nym nym-mixnode-server/latest
    if [ $? -eq 0 ]
    then
	 docker rm nym-${SERVICE}-server-old
	 echo "Container Removed ${SERVICE}"
    fi
    echo mixnode run container
elif [ "${SERVICE}" == "client" ]
then
    check_container=`docker ps -a | grep  "\bnym-${SERVICE}-server\b" | grep -v "\bnym-${SERVICE}-server\b"`
    docker stop nym-${SERVICE}-server
    docker rename nym-${SERVICE}-server nym-${SERVICE}-server-old > /dev/null
    docker run -d --name nym-client-server --cap-add=NET_ADMIN --cap-add=NET_RAW --network host  -v ${STORAGE}:/home/nym/.nym nym-client-server/latest
    if [ $? -eq 0 ]
    then
         docker rm nym-${SERVICE}-server-old
         echo "Container Removed ${SERVICE}"
    fi
    echo client run container
elif [ "${SERVICE}" == "requester" ]
then
    check_container=`docker ps -a | grep  "\bnym-${SERVICE}-server\b" | grep -v "\bnym-${SERVICE}-server\b"`
    docker stop nym-${SERVICE}-server
    docker rename nym-${SERVICE}-server nym-${SERVICE}-server-old 2> /dev/null
    docker run -d --name nym-requester-server --cap-add=NET_ADMIN --cap-add=NET_RAW --network host  -v ${STORAGE}:/home/nym/.nym nym-requester-server/latest
    if [ $? -eq 0 ]
    then
         docker rm nym-${SERVICE}-server-old
         echo "Container Removed ${SERVICE}"
    fi
    echo requester run container
else
   help;
fi
}

if [ "${OPTIONS}" == "build" ]
then 
    build;
elif [ "${OPTIONS}" ==  "container" ]
then
    service;
elif [ "${OPTIONS}" ==  "full-update" ]
then
   echo "Warning Systemd was not Active"
   full:update;
else
    help;
fi
```

Create File .env\_mixnode

```bash
#!/bin/bash

ID="Name Of Mixnode"
WALLET="Assign If was Running First Service"
```

Create File in  mixnode.sh bin/

```bash
#!/bin/bash
set -x

source .env_mixnode

IP=`curl ifconfig.me`
HOME=/home/nym

function config_init () {
if [ ! -f $HOME/.nym/mixnodes/$ID/data/private_sphinx.pem ] || [ ! -f $HOME/.nym/mixnodes/$ID/data/public_sphinx.pem ]; then
    ./nym-mixnode init --id $ID --host 0.0.0.0 --wallet-address $WALLET --announce-host $IP  &&  ./nym-mixnode run --id $ID
elif [ -f $HOME/.nym/mixnodes/${ID}/data/private_sphinx.pem ] || [ -f $HOME/.nym/mixnodes/${ID}/data/public_sphinx.pem ]; then
    RUST_BACKTRACE=1 ./nym-mixnode run --id $ID
fi
}
```

Download Mixnode

```
wget -O bin/nym-mixnode \
-c https://github.com/nymtech/nym/releases/download/nym-binaries-v1.1.2/nym-mixnode

chmod +x bin/nym-mixnode
```
