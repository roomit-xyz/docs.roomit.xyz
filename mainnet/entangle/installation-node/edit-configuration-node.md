---
description: Edit Configuration Blockchain
---

# Edit Configuration Node

{% hint style="info" %}
**Running in user** : _salinem_
{% endhint %}

#### Set Minimum Gas Price

```bash
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.0001aNGL \"/" $HOME/.entangled/config/app.toml
```

#### **Enable Prometheus Matrics**

```bash
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.entangled/config/config.toml
```

#### Edit Port Node

```bash
PROXY_APP="16508" #Example
RPC="16708" #Example
PROF_RPC="1108" #Example
P2P="16608" #Example
METRICS="16808" #Example
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${PROXY_APP}\"%" $HOME/.entangled/config/config.toml 
sed -i.bak -e "s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${RPC}\"%" $HOME/.entangled/config/config.toml 
sed -i.bak -e "s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${PROF_RPC}\"%" $HOME/.entangled/config/config.toml 
sed -i.bak -e "s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${P2P}\"%" $HOME/.entangled/config/config.toml 
sed -i.bak -e "s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${METRICS}\"%" $HOME/.entangled/config/config.toml

```

#### Edit Port App

```bash
API="1208" #Example
GRPC="1308" #Example
WEBGRPC="1408" #Example
sed -i.bak -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${API}\"%" $HOME/.entangled/config/app.toml
sed -i.bak -e "s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${GRPC}\"%" $HOME/.entangled/config/app.toml
sed -i.bak -e "s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${WEBGRPC}\"%" $HOME/.entangled/config/app.toml
```

#### Edit Peers

Peers Seed

<pre class="language-bash"><code class="lang-bash">

<strong>seeds="8e682c52253e8d892e47b5e6cc0672e435301038@142.132.156.99:4156,75e62df2ed4615cabe38a7b2d645e329a800be47@185.205.246.212:26656,563a7251dd8cff3257e8db114a10f6a918a08b51@34.95.30.202:26656,1731c1b2f3bc8316a74d645a3f045c0f1aba7bc5@94.76.242.68:26656,9d25b1205658df64b509c21bcb212159f13039e3@37.252.186.204:26656,fbe38cb0500221859e631a93d7f830babea8871c@94.76.242.57:26656,f54b8c7a47abfcf18241c76a5c9a1bc5782cf59e@185.162.251.21:26656,bed1c2d7f2466e90f3e703ecbe1dafe370bd3e14@88.99.102.36:26656,cc8bf40df39533f1867141c8d99d62dc7f2a1867@46.4.23.120:29656,841380487cf89030e9ae865dfb884cc703e6c2b2@94.76.242.98:26656"
</strong>sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" ${HOME}/.entangled/config/config.toml
</code></pre>

Peers Node

```bash
peers=""
sed -i 's|^persistent_peers *=.*|persistent_peers = "'$peers'"|' ${HOME}/.entangled/config/config.toml
```

#### Fast Sync

Make sure you have parameter in $HOME/.entangled/config/config.toml

```
fast_sync = true
version = "v0"
```
