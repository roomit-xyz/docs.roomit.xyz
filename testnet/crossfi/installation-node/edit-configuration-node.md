---
description: Edit Configuration Blockchain
---

# Edit Configuration Node

{% hint style="info" %}
**Running in user** : _salinem_
{% endhint %}

#### Set Minimum Gas Price

```bash
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"10000000000000mpx \"/" $HOME/.crossfid/config/app.toml
```

#### **Enable Prometheus Matrics**

```bash
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.crossfid/config/config.toml
```

#### Edit Port Node

```bash
PROXY_APP="26505" #Example
RPC="26705" #Example
PROF_RPC="2105" #Example
P2P="26605" #Example
METRICS="26805" #Example
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${PROXY_APP}\"%" $HOME/.crossfid/config/config.toml 
sed -i.bak -e "s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${RPC}\"%" $HOME/.crossfid/config/config.toml 
sed -i.bak -e "s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${PROF_RPC}\"%" $HOME/.crossfid/config/config.toml 
sed -i.bak -e "s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${P2P}\"%" $HOME/.crossfid/config/config.toml 
sed -i.bak -e "s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${METRICS}\"%" $HOME/.crossfid/config/config.toml

```

#### Edit Port App

```bash
API="2205" #Example
GRPC="2305" #Example
WEBGRPC="2405" #Example
sed -i.bak -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${API}\"%" $HOME/.crossfid/config/app.toml
sed -i.bak -e "s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${GRPC}\"%" $HOME/.crossfid/config/app.toml
sed -i.bak -e "s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${WEBGRPC}\"%" $HOME/.crossfid/config/app.toml
```

#### Edit Peers

Peers Seed

<pre class="language-bash"><code class="lang-bash">

<strong>seeds="c8333d73f10b6cc83a5a10dfa51a374366ebd56d@5.75.131.173:26656,814f50857ab67f64df7e447fc7933210e5b9b75b@51.178.92.69:15656,c90c4360be9ff903c4c58f4bb5a1e0322640616a@167.235.12.38:14656,dda09f9625cab3fb655c22ef85d756fc77132b9d@167.235.102.45:10956,551b0407fc19de1ad69d26738bc59b5eeb678454@88.99.254.62:20656,0ff3504db616694c120ad76bb2c798ae8dbdcf0b@31.220.77.7:26656,d4f0c95d11b86ac3f17cb56221edc35847096d4d@84.247.146.39:26656,b7787910d1ad7eaebaad853aa7d5d3a23bdb0dd7@46.4.108.72:26656,74eae92f5282c54d04c0d1a03026ac7aeec381ad@195.201.237.9:26656,87c02a2693538b1eb7c2b29f53b9a5a8c62536e1@162.55.90.36:43656,8747f58157f8ac0295dc27e09cd0fadacf2952dc@65.109.84.33:26056,26e6d0290d242a1857a2b764e6d03222c30d51d5@135.181.183.93:49656,de1e9221a35de41e08653ef0aca405c1f2ad500e@65.21.202.124:19656,35d54b42a85e589ddf30cebe629b8e3a8daa5e2b@65.109.30.150:36656,a48cd814b162a8f5af3717688d602b225f212f58@65.109.53.101:25656,66bdf53ec0c2ceeefd9a4c29d7f7926e136f114a@65.109.36.231:36656,90db72b787a8ece982e8a266abadfe0214a7d1d4@185.11.248.93:26656,2916e699b7c588d8826bea1a190c6c82dfe7d5eb@5.188.36.196:26656,0ee3bd9b426c0c9436e3de656f4bb7aa9f1d2d67@222.252.19.248:26656,b363ae7542eea55264bcf67906bf0e7118015d11@142.132.152.46:11656,95a04e2fc27ff7e66bbb8591da7323e0e395d28c@80.64.208.223:26656,7d4fec478e005b4c942f402418deb4319d062c58@167.71.54.98:26656,741e4f6997feafbcca4d86eb3bc76900992e79ad@51.81.242.223:26656,05166745247e9c94018dbf2c27802d43e180d6d1@135.181.5.232:26056,b88d969ba0e158da1b4066f5c17af9da68c52c7a@65.109.53.24:44656,140e43a6c4a6da90d04ec4f8116915584b941dc8@65.21.237.90:36656,352d61156f5cefd81bd2f65206db43b8226b7be3@57.128.63.22:26656,978527c2cdb0804a35e8e22505dc47f1006efa1d@148.113.17.55:26656,05a60482feaa3b7246dd0fd9fa1785131fac7175@176.9.53.27:26656,b0b01c08d7d4c6c2740cc5fe6ea74eb7fdde64f2@38.242.151.229:26656"
</strong>sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" ${HOME}/.crossfid/config/config.toml
</code></pre>

Peers Node

```bash
peers=""
sed -i 's|^persistent_peers *=.*|persistent_peers = "'$peers'"|' ${HOME}/.crossfid/config/config.toml
```

#### Fast Sync

Make sure you have parameter in $HOME/.crossfid/config/config.toml

```
fast_sync = true
version = "v0"
```
