# Installation Relayer

{% hint style="info" %}
Assume we have already done install gbt refer to [installation-validator.md](installation-validator.md "mention")
{% endhint %}

{% hint style="info" %}
**Running in user** : _salinem_
{% endhint %}

Test for Relayer

```bash
gbt relayer \
--ethereum-key 0xYOURKEYHERE
--cosmos-grpc https://gravitychain.io:9090
--ethereum-rpc https://eth.althea.net
--gravity-contract-address "0xa4108aA1Ec4967F8b52220a4f7e94A8201F2D906" \
```

Create Systemd Relayer

```bash
cd ${HOME}
mkdir -p systemd
cat > systemd/gravity-relayer.service << EOF
[Unit]
Description=Gravity bridge relayer
Requires=network.target

[Service]
Type=simple
User=salinem
Group=salinem
TimeoutStartSec=10s
Restart=always
RestartSec=10
ExecStart=/mainnet/salinem/bin/gbt relayer \
--ethereum-key "YOUR_ETHEREUM_KEYS_PRIVATE" \
--cosmos-grpc http://gravitychain.io:9090 \
--ethereum-rpc "YOUR ALCHEMY RPC" \
--gravity-contract-address "0xa4108aA1Ec4967F8b52220a4f7e94A8201F2D906"
Environment="HOME=/mainnet/salinem"

[Install]
WantedBy=default.target
EOF
```



Linking systemd init

```bash
sudo ln -sf /mainnet/salinem/systemd/gravity-relayer.service /etc/systemd/system/
```

Start Service

<pre class="language-bash"><code class="lang-bash"><strong>sudo systemctl daemon-reload
</strong><strong>sudo systemctl start gravity-relayer.service
</strong></code></pre>

