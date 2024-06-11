---
description: Edit Configuration Blockchain
---

# Edit Configuration Node

{% hint style="info" %}
**Running in user** : _salinem_
{% endhint %}

#### Set Minimum Gas Price

```bash
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.005uslf \"/" $HOME/.selfchain/config/app.toml
```

#### **Enable Prometheus Matrics**

```bash
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.selfchain/config/config.toml
```

#### Edit Port Node

```bash
PROXY_APP="16509" #Example
RPC="16709" #Example
PROF_RPC="1109" #Example
P2P="16609" #Example
METRICS="16809" #Example
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${PROXY_APP}\"%" $HOME/.selfchain/config/config.toml 
sed -i.bak -e "s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${RPC}\"%" $HOME/.selfchain/config/config.toml 
sed -i.bak -e "s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${PROF_RPC}\"%" $HOME/.selfchain/config/config.toml 
sed -i.bak -e "s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${P2P}\"%" $HOME/.selfchain/config/config.toml 
sed -i.bak -e "s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${METRICS}\"%" $HOME/.selfchain/config/config.toml

```

#### Edit Port App

```bash
API="1209" #Example
GRPC="1309" #Example
WEBGRPC="1409" #Example
sed -i.bak -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${API}\"%" $HOME/.selfchain/config/app.toml
sed -i.bak -e "s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${GRPC}\"%" $HOME/.selfchain/config/app.toml
sed -i.bak -e "s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${WEBGRPC}\"%" $HOME/.selfchain/config/app.toml
```

#### Edit Peers

Peers Seed

<pre class="language-bash"><code class="lang-bash">

<strong>seeds="c139f537755d5614a3ceaeb0f01b03be94e7ecb5@162.19.171.121:26656,6a3a0db2763d8222d00af55cbbe35824a39c8292@176.9.183.45:34656,790544e857cfe673cab570668131aa7ae2be7e5d@178.63.100.102:26656,7a9038d1efd34c7f3baea17d8822262a981568b1@217.182.136.79:30156,6ae10267d8581414b37553655be22297b2f92087@174.138.25.159:26656,b307b56b94bd3a02fcad5b6904464a391e13cf48@128.199.33.181:26656,71b8d630e7c3e31f2743fda68e6d3ac64f41cece@209.97.174.97:26656,f238d6a52578975198ceac2b0c2b004d49d5613f@88.198.5.77:31656,c87c1b17045b27fd14b13d7dbb3469a2248cb1f7@95.217.204.58:24356,637077d431f618181597706810a65c826524fd74@5.9.151.56:24356"
</strong>sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" ${HOME}/.selfchain/config/config.toml
</code></pre>

Peers Node

```bash
peers=""
sed -i 's|^persistent_peers *=.*|persistent_peers = "'$peers'"|' ${HOME}/.selfchain/config/config.toml
```

#### Fast Sync

Make sure you have parameter in $HOME/.selfchain/config/config.toml

```
fast_sync = true
version = "v0"
```
