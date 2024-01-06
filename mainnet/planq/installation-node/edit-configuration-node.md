---
description: Edit Configuration Blockchain
---

# Edit Configuration Node

{% hint style="info" %}
**Running in user** : _salinem_
{% endhint %}

#### Set Minimum Gas Price

```bash
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0aplanq\"/" $HOME/.planqd/config/app.toml
```

#### **Enable Prometheus Matrics**

```bash
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.planqd/config/config.toml
```

#### Edit Port Node&#x20;

```bash
PROXY_APP="16503"
RPC="1673"
PROF_RPC="1103"
P2P="16603"
METRICS="16803"
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${PROXY_APP}\"%" $HOME/.planqd/config/config.toml 
sed -i.bak -e "s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${RPC}\"%" $HOME/.planqd/config/config.toml 
sed -i.bak -e "s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${PROF_RPC}\"%" $HOME/.planqd/config/config.toml 
sed -i.bak -e "s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${P2P}\"%" $HOME/.planqd/config/config.toml 
sed -i.bak -e "s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${METRICS}\"%" $HOME/.planqd/config/config.toml

```

#### Edit Port App

```bash
API="1203"
GRPC="1303"
WEBGRPC="1403"
sed -i.bak -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${API}\"%" $HOME/.planqd/config/app.toml
sed -i.bak -e "s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${GRPC}\"%" $HOME/.planqd/config/app.toml
sed -i.bak -e "s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${WEBGRPC}\"%" $HOME/.planqd/config/app.toml
```

#### Edit Peers

Peers Seed

<pre class="language-bash"><code class="lang-bash"><strong>seeds="dd2f0ceaa0b21491ecae17413b242d69916550ae@135.125.247.70:26656,0525de7e7640008d2a2e01d1a7f6456f28f3324c@51.79.142.6:26656,21432722b67540f6b366806dff295849738d7865@139.99.223.241:26656"
</strong>sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" ${HOME}/.planqd/config/config.toml
</code></pre>

Peers Node

```bash
peers="3eb12284b7fb707490b8adfda6fa7d94e2fa5cd9@planq.p2p.roomit.xyz:16603,dd2f0ceaa0b21491ecae17413b242d69916550ae@135.125.247.70:26656,0525de7e7640008d2a2e01d1a7f6456f28f3324c@51.79.142.6:26656,21432722b67540f6b366806dff295849738d7865@139.99.223.241:26656,7c10b1a106a512976e8d71effe5c086327458eef@35.200.183.35:26656,b76abe67188be594e17d6e25c7231b027c8bd324@34.175.180.219:26656,60d86f728656b170956f826f54139b8bd6d16205@82.65.197.168:26656"
sed -i 's|^persistent_peers *=.*|persistent_peers = "'$peers'"|' ${HOME}/.planqd/config/config.toml
```

#### Fast Sync

Make sure you have parameter in $HOME/.planqd/config/config.toml

```
fast_sync = true
version = "v0"
```
