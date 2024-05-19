---
description: Edit Configuration Blockchain
---

# Edit Configuration Node

{% hint style="info" %}
**Running in user** : _salinem_
{% endhint %}

#### Set Minimum Gas Price

```bash
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0unibi\"/" $HOME/.nibid/config/app.toml
```

#### **Enable Prometheus Matrics**

```bash
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.nibid/config/config.toml
```

#### Edit Port Node&#x20;

```bash
PROXY_APP="16506"
RPC="1673"
PROF_RPC="1106"
P2P="16606"
METRICS="16806"
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${PROXY_APP}\"%" $HOME/.nibid/config/config.toml 
sed -i.bak -e "s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${RPC}\"%" $HOME/.nibid/config/config.toml 
sed -i.bak -e "s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${PROF_RPC}\"%" $HOME/.nibid/config/config.toml 
sed -i.bak -e "s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${P2P}\"%" $HOME/.nibid/config/config.toml 
sed -i.bak -e "s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${METRICS}\"%" $HOME/.nibid/config/config.toml

```

#### Edit Port App

```bash
API="1206"
GRPC="1306"
WEBGRPC="1406"
sed -i.bak -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${API}\"%" $HOME/.nibid/config/app.toml
sed -i.bak -e "s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${GRPC}\"%" $HOME/.nibid/config/app.toml
sed -i.bak -e "s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${WEBGRPC}\"%" $HOME/.nibid/config/app.toml
```

#### Edit Peers

Peers Seed

<pre class="language-bash"><code class="lang-bash"><strong>seeds=""
</strong>sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" ${HOME}/.nibid/config/config.toml
</code></pre>

Peers Node

```bash
peers=""
sed -i 's|^persistent_peers *=.*|persistent_peers = "'$peers'"|' ${HOME}/.nibid/config/config.toml
```

#### Fast Sync

Make sure you have parameter in $HOME/.nibid/config/config.toml

```
fast_sync = true
version = "v0"
```
