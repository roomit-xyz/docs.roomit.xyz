---
description: Install Orchestrator
---

# Installation Orcehstrator



{% hint style="info" %}
**Validator Funds key**: This is the key you submitted for genesis it starts with `gravity1` and contains your funds



**Validator Operator Key:** This is a key that will be generated with your gentx, it starts with `gravityvaloper1` and actually signs your validators blocks



**Gravity Orchestrator Cosmos Key:** This is a key that will be used on the Cosmos side of Gravity bridge to submit Oracle transactions and Ethereum signatures. This address will be actively used by Gravity bridge to send many hundreds of messages during normal day to day operation of an active bridge. You will be generating this key to register as part of your gentx.



**Gravity Orchestrator Ethereum Key:** This is an Ethereum key this is the key that represents your validators voting power on Ethereum in the `Gravity.sol` contract. In short this key secures the Gravity Bridge funds on Ethereum. This key will _not_ be actively used to submit messages to Ethereum unless you chose to relay in addition to validate. Like the Gravity Orchestrator Cosmos Key you will be generating this key here and registering it as part of your gentx.
{% endhint %}

{% hint style="info" %}
**Running in user** : _salinem_
{% endhint %}

Create RPC in [Alchemy Etherum Mainnet](https://alchemy.com/)

If you lose your Gravity delegate keys you will have to unbond and create a new validator because it's not possible to rotate them. So store all output of the following commands in a safe place.

```bash
gravity eth_keys add
gravity keys add <Your Gravity Orchestrator Cosmos Key Name>
```

Once we have generated our keys we will also set them in our Orchestrator right away, this reduces the risk of confusion as your validator starts and you need these keys to submit Gravity bridge signatures via your orchestrator.

```bash
gbt init
gbt keys set-ethereum-key --key Gravity Orchestrator Ethereum Key
gbt keys set-orchestrator-key --phrase "Gravity Orchestrator Cosmos Key"
```

Finally we have now generated the keys, stored them in a safe place, set those keys in the gbt process. We can now use gbt to register these keys to the validator key. This process is not reversable, if you lose the keys you generated in the last steps you will have to create a new validator.

```bash
gbt keys register-orchestrator-address --validator-phrase "your validator key phrase"
```

Create Systemd

{% hint style="info" %}
```
YOUR_ALCHEMY_URL is https://eth-mainnet.g.alchemy.com/v2/xxxxxxxxxxx
```
{% endhint %}

<pre class="language-bash"><code class="lang-bash"><strong>cd ${HOME}
</strong><strong>mkdir systemd
</strong><strong>cat > systemd/gravity-orchestrator.service&#x3C;&#x3C;EOF
</strong><strong>[Unit]
</strong>Description=Gravity bridge orchestrator
Requires=network.target

[Service]
Type=simple
User=salinem
Group=salinem
TimeoutStartSec=10s
Restart=always
RestartSec=10
ExecStart=/mainnet/salinem/bin/gbt orchestrator\
--fees 0ugraviton \
--cosmos-grpc=http://gravitychain.io:9090 \
--ethereum-rpc $YOUR_ALCHEMY_URL
Environment="HOME=/mainnet/salinem"

[Install]
WantedBy=default.target
EOF

</code></pre>

Linking systemd init

```bash
sudo ln -sf /mainnet/salinem/systemd/gravity-orchestrator.service /etc/systemd/system/
```

Start Service

<pre class="language-bash"><code class="lang-bash"><strong>sudo systemctl daemon-reload
</strong><strong>sudo systemctl start gravity-orchestrator.service
</strong></code></pre>

