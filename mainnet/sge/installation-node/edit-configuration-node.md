---
description: Edit Configuration Blockchain
---

# Edit Configuration Node

{% hint style="info" %}
**Running in user** : _salinem_
{% endhint %}

#### Set Minimum Gas Price

```bash
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.025usge \"/" $HOME/.sge/config/app.toml
```

#### **Enable Prometheus Matrics**

```bash
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.sge/config/config.toml
```

#### Edit Port Node

```bash
PROXY_APP="16505" #Example
RPC="16705" #Example
PROF_RPC="1105" #Example
P2P="16605" #Example
METRICS="16805" #Example
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${PROXY_APP}\"%" $HOME/.sge/config/config.toml 
sed -i.bak -e "s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${RPC}\"%" $HOME/.sge/config/config.toml 
sed -i.bak -e "s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${PROF_RPC}\"%" $HOME/.sge/config/config.toml 
sed -i.bak -e "s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${P2P}\"%" $HOME/.sge/config/config.toml 
sed -i.bak -e "s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${METRICS}\"%" $HOME/.sge/config/config.toml

```

#### Edit Port App

```bash
API="1205" #Example
GRPC="1305" #Example
WEBGRPC="1405" #Example
sed -i.bak -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${API}\"%" $HOME/.sge/config/app.toml
sed -i.bak -e "s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${GRPC}\"%" $HOME/.sge/config/app.toml
sed -i.bak -e "s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${WEBGRPC}\"%" $HOME/.sge/config/app.toml
```

#### Edit Peers

Peers Seed

<pre class="language-bash"><code class="lang-bash">

<strong>seeds="c2b06ac64284267dde6627db93eef6c87bf81162@65.108.232.168:19056,304535618b71c2fe217fe771c745443ea3d7815e@65.108.0.94:17756,88f341a9670494c3d529934dc578eec1b00f4aa1@141.94.168.85:26656,c298f3b9d90a5258d48a97ab0577de734a4052dc@65.108.204.209:17756,49d3176741019c4fa911183725981d44c6d51c44@3.34.129.58:26656,423b0dafd7a6b9ea5406d5a0fbae667d9d7ffc11@142.132.248.253:22656,62b3744b7b703de5d946301aa6ea092cda89e02f@213.239.207.175:13156,9d6916344cea096b7adc86b67faf65f2815f09d3@167.235.39.5:60956,ba0a167567c7e08f4bb1e25ec24e42a85b07a0c5@148.113.20.208:17756,053d96f525122d4e20c20b6722d38b367cb04210@65.109.88.254:32656,d91ff3e80de5e07303efb74c6daa02d981b624c4@213.190.31.82:17756,a51476d949df940e636f6d072b14b42aac3a16a8@213.246.45.16:5656,31fa7b0ac148384262dc56c5e90a23e7d48f471d@167.179.79.232:26656,3247b9d019bca2e86e864f5abe8c9b28c2064548@95.165.89.222:24136,3bea9b46a5b067ccc9208e8952e684d37e18e5b0@141.164.56.119:26656,401a4986e78fe74dd7ead9363463ba4c704d8759@38.146.3.183:17756,fd55622051218695a44e1b23aa4347287d70a26d@176.9.92.135:46656,2fd9042e02321cccf513731d6a9bb70c3b12b62d@3.39.25.1:26656,ff69cca1c97a20057c571862e242635e2c460097@141.164.61.145:26656,c902c5873ef611cda24bf4f2be2229171223afdd@3.39.223.174:26656,26238cbb6bf285816bd06ca946b190e7248c389c@46.38.232.86:22656,6619d41e0a1be47c4a79a5438b81793f06ffbc86@65.21.226.187:60556,17da9d2fea9d6d431d390c3b9575547d8881da2b@185.16.39.190:11156,7258d8c7880167fca502592b8d64110d60e99a6b@65.108.232.180:17756,fe527359b6b6c5ad9cc6e2f6ed3af46018b29e15@136.243.36.60:17756,13d370cf706e0cbcbed962f7f5585efead848132@158.69.125.73:10456,d45e4471ed399bfa21c3f5d49c8f9ec0dc1940a7@150.230.137.157:26656,3fc703341935b9356addfe7b3aad8991d9c8a923@148.113.20.207:17756,08db9d08ff534ed65a1c7078e2406d18ff1cc385@51.210.223.84:17756,fa5c3f970a30023c5793cade0fa449d5c8c8e6d5@89.168.24.105:26656"
</strong>sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" ${HOME}/.sge/config/config.toml
</code></pre>

Peers Node

```bash
peers=""
sed -i 's|^persistent_peers *=.*|persistent_peers = "'$peers'"|' ${HOME}/.sge/config/config.toml
```

#### Fast Sync

Make sure you have parameter in $HOME/.sge/config/config.toml

```
fast_sync = true
version = "v0"
```
