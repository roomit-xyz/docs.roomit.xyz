---
description: Installation Node mysterium
---

# Installation Mysterium

{% hint style="info" %}
Ensure you have added user salinem [user-and-group-management.md](../../security/user-and-group-management.md "mention")

Ensure Docker Installed
{% endhint %}

Create file myst-init.sh

```
#!/bin/bash


NAME="myst-01"
DATABASE="$(pwd)/mysterium-node"

check_network=`docker network ls| grep  myst_network | head -n1 |wc -l`
if ! [ $check_network == 1 ]
then
  docker network create --driver bridge myst_network
else
  echo "Network Exist"
fi
docker pull mysteriumnetwork/myst && docker run --privileged --cap-add=ALL  --network myst_network   -d --dns 8.8.8.8 -p 127.0.0.1:4449:4449/tcp  --device /dev/net/tun:/dev/net/tun --name ${NAME} -v /lib/modules:/lib/modules  -v $DATABASE:/var/lib/mysterium-node  --restart unless-stopped mysteriumnetwork/myst:latest service --agreed-terms-and-conditions

```

