---
description: Script Updates Ports
---

# Update Ports

```bash
#!/bin/bash
#
# RoomIT
# https://roomit.xyz
# If this script useful and you will visit cikarang indonesia, 
# Let's drink coffee and talk about blockchain
#

##### Global Env
CONF_BLOCKCHAIN=".comdex"
HOME_VALIDATOR=`pwd`
CHAIN_ID="comdex-1"

##### config.toml
PROXY_APP="16501"
RPC="16701"
PROF_RPC="1101"
P2P="16601"
METRICS="16801"

##### app.toml
API="1201"
GRPC="1301"
WEBGRPC="1401"

PORT_ARRAY=("${PROXY_APP}" "${RPC}" "${PROF_RPC}" "${P2P}" "${METRICS}" "${API}" "${GRPC}" "${WEBGRPC}")

for port in  ${PORT_ARRAY[@]}
do
   check_port=`ss -tulpn | grep ${port} | awk '{print $5}' | awk -F":" '{print $2}' | tr -d " " | wc -l`
   if [ ${port} -eq 1 ]
   then
      echo "PORT was exist"
      exit 1;
    else
      echo "Port ${port} - OK"
    fi
done




sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${PROXY_APP}\"%" ${HOME_VALIDATOR}/${CONF_BLOCKCHAIN}/config/config.toml 
sed -i.bak -e "s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${RPC}\"%" ${HOME_VALIDATOR}/${CONF_BLOCKCHAIN}/config/config.toml 
sed -i.bak -e "s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${PROF_RPC}\"%" ${HOME_VALIDATOR}/${CONF_BLOCKCHAIN}/config/config.toml 
sed -i.bak -e "s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${P2P}\"%" ${HOME_VALIDATOR}/${CONF_BLOCKCHAIN}/config/config.toml 
sed -i.bak -e "s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${METRICS}\"%" ${HOME_VALIDATOR}/${CONF_BLOCKCHAIN}/config/config.toml
sed -i.bak -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${API}\"%" ${HOME_VALIDATOR}/${CONF_BLOCKCHAIN}/config/app.toml
sed -i.bak -e "s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${GRPC}\"%" ${HOME_VALIDATOR}/${CONF_BLOCKCHAIN}/config/app.toml
sed -i.bak -e "s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${WEBGRPC}\"%" ${HOME_VALIDATOR}/${CONF_BLOCKCHAIN}/config/app.toml
sed -i.bak -e "s%^node = \"tcp://localhost:26657\"%address = \"tcp://0.0.0.0:${RPC}\"%" ${HOME_VALIDATOR}/${CONF_BLOCKCHAIN}/config/client.toml


```
