---
description: Edit Configuration Blockchain
---

# Edit Configuration Node

{% hint style="info" %}
**Running in user** : _salinem_
{% endhint %}

#### Set Minimum Gas Price

```bash
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.025abcx\"/" $HOME/.blockxd/config/app.toml
```

#### **Enable Prometheus Matrics**

```bash
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.blockxd/config/config.toml
```

#### Edit Port Node&#x20;

```bash
PROXY_APP="16507"
RPC="16707"
PROF_RPC="1107"
P2P="16607"
METRICS="16807"
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:${PROXY_APP}\"%" $HOME/.blockxd/config/config.toml 
sed -i.bak -e "s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:${RPC}\"%" $HOME/.blockxd/config/config.toml 
sed -i.bak -e "s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:${PROF_RPC}\"%" $HOME/.blockxd/config/config.toml 
sed -i.bak -e "s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:${P2P}\"%" $HOME/.blockxd/config/config.toml 
sed -i.bak -e "s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":${METRICS}\"%" $HOME/.blockxd/config/config.toml

```

#### Edit Port App

```bash
API="1207"
GRPC="1307"
WEBGRPC="1407"
sed -i.bak -e "s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:${API}\"%" $HOME/.blockxd/config/app.toml
sed -i.bak -e "s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:${GRPC}\"%" $HOME/.blockxd/config/app.toml
sed -i.bak -e "s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:${WEBGRPC}\"%" $HOME/.blockxd/config/app.toml
```

#### Edit Peers

Peers Seed

<pre class="language-bash"><code class="lang-bash"><strong>seeds="91c24aac8da5df96435b2af2984a6a5b5b00b54e@mainnet-seed.konsortech.xyz:39156,479dfa1948f49b08810cd16bf6c2d3256ae85423@137.184.7.64:26656,e15f4d31281036c69fa17269d9b26ff8733511c6@147.182.238.235:26656,9b84b33d44a880a520006ae9f75ef030b259cbaf@137.184.38.212:26656,85d0069266e78896f9d9e17915cdfd271ba91dfd@146.190.153.165:26656"
</strong>sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" ${HOME}/.blockxd/config/config.toml
</code></pre>

Peers Node

```bash
peers="adcd9c90cc9fba509fb283e365ecd31bd5c37ff5@49.13.166.213:26656,72639ce4ce7e0260d7ae129e6acc07dcb54d6af1@167.235.102.45:20656,724b268dbb274e7d4b26503129604a968c9e226b@37.120.189.81:26656,8ebf5e70dad7268a66a9198dbe9006f9140415b6@217.182.211.81:26656,bbe679ddc774dc91b962985c7339a2e7934b8451@207.180.250.5:26656,bc152258668e673a3b63f964fa75afdd478078c7@185.246.85.48:39656,ed5384bd984a04f19aeb7e17699c061bffd16c41@88.99.59.227:11632,34d08633547fc406095ff6d730fdfe65d34b96d0@158.69.125.73:11356,e15f4d31281036c69fa17269d9b26ff8733511c6@147.182.238.235:26656,dc240d568509fa275cb870b93de4db1869d7187a@5.78.103.187:26656,85d0069266e78896f9d9e17915cdfd271ba91dfd@146.190.153.165:26656,9b84b33d44a880a520006ae9f75ef030b259cbaf@137.184.38.212:26656"
sed -i 's|^persistent_peers *=.*|persistent_peers = "'$peers'"|' ${HOME}/.blockxd/config/config.toml
```

#### Fast Sync

Make sure you have parameter in $HOME/.blockxd/config/config.toml

```
fast_sync = true
version = "v0"
```
