---
description: Edit Configuration Blockchain
---

# Edit Configuration Node

{% hint style="info" %}
**Running in user** : _salinem_
{% endhint %}

#### Set Minimum Gas Price

```bash
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0atplanq \"/" $HOME/.planqd/config/app.toml
```

#### **Enable Prometheus Matrics**

```bash
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.planqd/config/config.toml
```

#### Edit Port Node

```bash
PROXY_APP="16503" #Example
RPC="16703" #Example
PROF_RPC="1103" #Example
P2P="16603" #Example
METRICS="16803" #Example
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${PROXY_APP}\"%" $HOME/.planqd/config/config.toml 
sed -i.bak -e "s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${RPC}\"%" $HOME/.planqd/config/config.toml 
sed -i.bak -e "s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${PROF_RPC}\"%" $HOME/.planqd/config/config.toml 
sed -i.bak -e "s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${P2P}\"%" $HOME/.planqd/config/config.toml 
sed -i.bak -e "s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${METRICS}\"%" $HOME/.planqd/config/config.toml

```

#### Edit Port App

```bash
API="1203" #Example
GRPC="1303" #Example
WEBGRPC="1403" #Example
sed -i.bak -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${API}\"%" $HOME/.planqd/config/app.toml
sed -i.bak -e "s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${GRPC}\"%" $HOME/.planqd/config/app.toml
sed -i.bak -e "s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${WEBGRPC}\"%" $HOME/.planqd/config/app.toml
```

#### Edit Peers

Peers Seed

<pre class="language-bash"><code class="lang-bash">

<strong>seeds=""
</strong>sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" ${HOME}/.planqd/config/config.toml
</code></pre>

Peers Node

```bash
peers=""
sed -i 's|^persistent_peers *=.*|persistent_peers = "'$peers'"|' ${HOME}/.planqd/config/config.toml
```

#### Fast Sync

Make sure you have parameter in $HOME/.planqd/config/config.toml

```
fast_sync = true
version = "v0"
```
