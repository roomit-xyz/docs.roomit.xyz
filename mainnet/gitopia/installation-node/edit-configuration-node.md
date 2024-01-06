---
description: Edit Configuration Blockchain
---

# Edit Configuration Node

{% hint style="info" %}
**Running in user** : _salinem_
{% endhint %}

#### Set Minimum Gas Price

```bash
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0ulore\"/" $HOME/.gitopia/config/app.toml
```

#### **Enable Prometheus Matrics**

```bash
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.gitopia/config/config.toml
```

#### Edit Port Node

```bash
PROXY_APP="16501"
RPC="16701"
PROF_RPC="1101"
P2P="16601"
METRICS="16801"
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${PROXY_APP}\"%" $HOME/.gitopia/config/config.toml 
sed -i.bak -e "s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${RPC}\"%" $HOME/.gitopia/config/config.toml 
sed -i.bak -e "s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${PROF_RPC}\"%" $HOME/.gitopia/config/config.toml 
sed -i.bak -e "s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${P2P}\"%" $HOME/.gitopia/config/config.toml 
sed -i.bak -e "s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${METRICS}\"%" $HOME/.gitopia/config/config.toml

```

#### Edit Port App

```bash
API="1201"
GRPC="1301"
WEBGRPC="1401"
sed -i.bak -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${API}\"%" $HOME/.gitopia/config/app.toml
sed -i.bak -e "s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${GRPC}\"%" $HOME/.gitopia/config/app.toml
sed -i.bak -e "s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${WEBGRPC}\"%" $HOME/.gitopia/config/app.toml
```

#### Edit Peers

Peers Seed

<pre class="language-bash"><code class="lang-bash">

<strong>seeds="babc3f3f7804933265ec9c40ad94f4da8e9e0017@seed.rhinostake.com:11356,ade4d8bc8cbe014af6ebdf3cb7b1e9ad36f412c0@seeds.polkachu.com:11356,a4a69a62de7cb0feb96c239405aa247a5a258739@seeds.cros-nest.com:57656"
</strong>sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" ${HOME}/.gitopia/config/config.toml
</code></pre>

Peers Node

```bash
peers="4cf66531681c92f15c95c25bd1bff524f9dca35e@65.109.154.181:26656,b2f764694d52e09793d68259d584ece0c194b6fe@65.108.229.93:26656,082e95b5d5351e68dcfb24dff802f9064cfd5a4c@65.109.92.241:51056,a94aec7233f9fec2b2de4b5c9dab6ad979820b3d@65.109.104.118:60756,a0ebd1e5845148c47451452047c7c99621da195e@65.109.96.93:60556,4adfa5889675e1e91ea4459e15ff4a0ba53e7828@65.108.224.156:19656,12f6b84a23b054a6591c647c2a4456c40af65cce@5.9.147.22:24657,88497ab3bbbcc1e8545771f45020e738bcce590f@95.165.89.222:24136,abca18ed112719b4f0a23932797dba2733f0fd44@23.88.5.169:25656,976d95adec7f0d7fda4464df019fa538fa0bb4ce@144.76.97.251:44656,ffd761a9e0d86609de6dae5935f99451694051a9@34.28.130.17:26656,5b2df98ad73a0a81a5bd31da4489a9236a7d7a99@65.21.91.160:26867,712dd67b7abe08577d394e90a4930492c8f7d2ee@65.108.124.219:41656,dee34ce3265a8901b52edfef8914530e4a9d05af@gitopia.sergo.dev:12283,c35eb6124591bad21673e8d802898faa18e0352a@65.109.29.150:36656,f9b892ea2e8ed8aa83f7b98e7e47371c23b01924@213.239.207.175:36656,8105128854785c428cff7aeed4f8b44a0a478252@provider.boxedcloud.net:31527,8e42db619abe34afe8cb39d4a2d04ae5db5bdaaf@141.94.139.233:26656,de34c6491557c59bc5d73631fb73bf05cd726e3e@142.132.202.50:37656,50b42bb809f445eb59db844ef6550658ef51a391@79.143.179.196:43656,a5233e4359a39e09d7b261c200cdc014bbef76ad@65.108.8.247:11356,2330fa28b8613786c741778d057616e3b91a8ae9@95.217.192.230:21656,a0b6c89b4fe0f455a027080103bffd001f3b6248@65.21.134.202:26356,abd217aa49d5ee86c271d04feef2cf4c97ff8d55@gitopia.p2p.roomit.xyz:16601,0e9f303834a5d1f3be0babd5466725b3609ebc82@65.21.141.246:28656,cf721cc0ae7b6cf9c58784ccfc87f6f621a09ffb@65.109.99.68:26656,11879f38e16e1723ef70950f5222ec78dde7e62f@65.109.17.23:56240,fd6a538800c2acb937f9fc45268704361af1459f@65.109.116.204:12256,b35d46fcfc1e4cfa943a299fcb39853e15e94d8b@81.30.157.35:14656"
sed -i 's|^persistent_peers *=.*|persistent_peers = "'$peers'"|' ${HOME}/.gitopia/config/config.toml
```

#### Fast Sync

Make sure you have parameter in $HOME/.gitopia/config/config.toml

```
fast_sync = true
version = "v0"
```
