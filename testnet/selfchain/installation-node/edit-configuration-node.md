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
PROXY_APP="26503" #Example
RPC="26703" #Example
PROF_RPC="2103" #Example
P2P="26603" #Example
METRICS="26803" #Example
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${PROXY_APP}\"%" $HOME/.selfchain/config/config.toml 
sed -i.bak -e "s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${RPC}\"%" $HOME/.selfchain/config/config.toml 
sed -i.bak -e "s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${PROF_RPC}\"%" $HOME/.selfchain/config/config.toml 
sed -i.bak -e "s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${P2P}\"%" $HOME/.selfchain/config/config.toml 
sed -i.bak -e "s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${METRICS}\"%" $HOME/.selfchain/config/config.toml

```

#### Edit Port App

```bash
API="2203" #Example
GRPC="2303" #Example
WEBGRPC="2403" #Example
sed -i.bak -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${API}\"%" $HOME/.selfchain/config/app.toml
sed -i.bak -e "s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${GRPC}\"%" $HOME/.selfchain/config/app.toml
sed -i.bak -e "s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${WEBGRPC}\"%" $HOME/.selfchain/config/app.toml
```

#### Edit Peers

Peers Seed

<pre class="language-bash"><code class="lang-bash">

<strong>seeds="3a6608f456836bdd69d65ae2cf5854f8cd76c27b@3.147.222.52:26656,c0dbddb16c0060c99f243b80b20c9bccce71a7bf@3.21.176.122:26656,f8ac3f2b6b07bf42790b7a0c553715ebdd7bfee6@152.53.108.188:23356,406740ae6e321cb9d42397a609c9b597248330a1@144.76.97.251:41656,54bc6768af48108d7c50344362165c7870d89275@37.27.52.25:13656,447c1f962696fcdd6c31ca1ae073c97c9cb15f58@3.138.56.49:26656,c6835dc6ba0b1fcc86602373fddb451fdca8890b@5.9.87.231:60756"
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
