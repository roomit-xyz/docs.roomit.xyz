---
description: Edit Configuration Blockchain
---

# Edit Configuration Node

{% hint style="info" %}
**Running in user** : _salinem_
{% endhint %}

#### Set Minimum Gas Price

```
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0ugraviton\"/" $HOME/.gravity/config/app.toml
```

#### **Enable Prometheus Matrics**

```
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.gravity/basconfig/config.toml
```

#### Edit Port Node&#x20;

```bash
PROXY_APP="16500"
RPC="16700"
PROF_RPC="1100"
P2P="16600"
METRICS="16800"
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${PROXY_APP}\"%" $HOME/.gravity/config/config.toml 
sed -i.bak -e "s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${RPC}\"%" $HOME/.gravity/config/config.toml 
sed -i.bak -e "s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${PROF_RPC}\"%" $HOME/.gravity/config/config.toml 
sed -i.bak -e "s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${P2P}\"%" $HOME/.gravity/config/config.toml 
sed -i.bak -e "s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${METRICS}\"%" $HOME/.gravity/config/config.toml

```

#### Edit Port App

```bash
API="1200"
# ROSETA="1600"
GRPC="1300"
WEBGRPC="1400"
sed -i.bak -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${API}\"%" $HOME/.gravity/config/app.toml
# sed -i.bak -e "s%^address = \":8080\"%address = \":${ROSETA}\"%" $HOME/.gravity/config/app.toml
sed -i.bak -e "s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${GRPC}\"%" $HOME/.gravity/config/app.toml
sed -i.bak -e "s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${WEBGRPC}\"%" $HOME/.gravity/config/app.toml
```
