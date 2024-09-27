---
description: Edit Configuration Blockchain
---

# Edit Configuration Node

{% hint style="info" %}
**Running in user** : _salinem_
{% endhint %}

#### Set Minimum Gas Price

```bash
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0uarkeo \"/" $HOME/.arkeo/config/app.toml
```

#### **Enable Prometheus Matrics**

```bash
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.arkeo/config/config.toml
```

#### Edit Port Node

```bash
PROXY_APP="26500" #Example
RPC="26700" #Example
PROF_RPC="2100" #Example
P2P="26600" #Example
METRICS="26800" #Example
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${PROXY_APP}\"%" $HOME/.arkeo/config/config.toml 
sed -i.bak -e "s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${RPC}\"%" $HOME/.arkeo/config/config.toml 
sed -i.bak -e "s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${PROF_RPC}\"%" $HOME/.arkeo/config/config.toml 
sed -i.bak -e "s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${P2P}\"%" $HOME/.arkeo/config/config.toml 
sed -i.bak -e "s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${METRICS}\"%" $HOME/.arkeo/config/config.toml

```

#### Edit Port App

```bash
API="2200" #Example
GRPC="2300" #Example
WEBGRPC="2400" #Example
sed -i.bak -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${API}\"%" $HOME/.arkeo/config/app.toml
sed -i.bak -e "s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${GRPC}\"%" $HOME/.arkeo/config/app.toml
sed -i.bak -e "s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${WEBGRPC}\"%" $HOME/.arkeo/config/app.toml
```

#### Edit Peers

Peers Seed

<pre class="language-bash"><code class="lang-bash">

<strong>seeds=""
</strong>sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" ${HOME}/.arkeo/config/config.toml
</code></pre>

Peers Node

```bash
peers=""
sed -i 's|^persistent_peers *=.*|persistent_peers = "'$peers'"|' ${HOME}/.arkeo/config/config.toml
```

#### Fast Sync

Make sure you have parameter in $HOME/.arkeo/config/config.toml

```
fast_sync = true
version = "v0"
```
