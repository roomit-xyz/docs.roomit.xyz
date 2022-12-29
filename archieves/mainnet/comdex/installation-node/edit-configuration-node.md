---
description: Edit Configuration Blockchain
---

# Edit Configuration Node

{% hint style="info" %}
**Running in user** : _salinem_
{% endhint %}

#### Set Minimum Gas Price

```
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.025ucmdx\"/" $HOME/.comdex/config/app.toml
```

#### **Enable Prometheus Matrics**

```
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.comdex/basconfig/config.toml
```

#### Edit Port Node&#x20;

```bash
PROXY_APP="16501"
RPC="1671"
PROF_RPC="1101"
P2P="16601"
METRICS="16801"
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${PROXY_APP}\"%" $HOME/.comdex/config/config.toml 
sed -i.bak -e "s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${RPC}\"%" $HOME/.comdex/config/config.toml 
sed -i.bak -e "s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${PROF_RPC}\"%" $HOME/.comdex/config/config.toml 
sed -i.bak -e "s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${P2P}\"%" $HOME/.comdex/config/config.toml 
sed -i.bak -e "s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${METRICS}\"%" $HOME/.comdex/config/config.toml

```

#### Edit Port App

```bash
API="1201"
GRPC="1301"
WEBGRPC="1401"
sed -i.bak -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${API}\"%" $HOME/.comdex/config/app.toml
sed -i.bak -e "s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${GRPC}\"%" $HOME/.comdex/config/app.toml
sed -i.bak -e "s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${WEBGRPC}\"%" $HOME/.comdex/config/app.toml
```
