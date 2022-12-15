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
#
##### Global Env
HOME_VALIDATOR="/mainnet/salinem"
CONF_BLOCKCHAIN=".gravity"
HOME_VALIDATOR=`pwd`


##### config.toml
PROXY_APP="26500"
RPC="26700"
PROF_RPC="2100"
P2P="26600"
METRICS="26800"

##### app.toml
API="2200"
GRPC="2300"
WEBGRPC="2400"



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
