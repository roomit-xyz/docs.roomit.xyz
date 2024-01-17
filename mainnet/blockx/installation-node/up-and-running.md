---
description: How to start running blockx
---

# Up And Running

{% hint style="info" %}
**Running in user** (Assume) : _salinem_
{% endhint %}

{% hint style="info" %}
Ensure you have add sudoers, refer [sudo-management.md](../../../security/sudo-management.md "mention")
{% endhint %}

#### Create Systemd

Create file in ${HOME}/systemd/blockx-node.service

```bash
export HOME="/mainnet/salinem"
cat > ${HOME}/systemd/blockx-node.service  <<EOF
[Unit]
Description=blockx node service
After=network-online.target

[Service]
User=blockx
ExecStart=${HOME}/bin/blockxd start 
Restart=on-failure
RestartSec=3
LimitNOFILE=500000
LimitNPROC=500000
Environment="HOME=${HOME}"


[Install]
WantedBy=multi-user.target

EOF
```

Linking to Systemd

```bash
ln -sf ${HOME}/systemd/blockx-node.service /etc/systemd/system/
```

Reload Daemon

```bash
sudo systemctl daemon-reload
```

Enable Service when booting

```bash
sudo systemctl enable blockx-node.service
```

Start Service

```bash
sudo systemctl start blockx-node.service
```

Check Service

```bash
sudo systemctl status blockx-node.service
sudo journalctl -fu blockx-node.service
```
