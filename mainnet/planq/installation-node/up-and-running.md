---
description: How to start running planq
---

# Up And Running

{% hint style="info" %}
**Running in user** (Assume) : _salinem_
{% endhint %}

{% hint style="info" %}
Ensure you have add sudoers, refer [sudo-management.md](../../../security/sudo-management.md "mention")
{% endhint %}

#### Create Systemd

Create file in ${HOME}/systemd/planq-node.service

```bash
export HOME="/mainnet/salinem"
cat > ${HOME}/systemd/planqd-node.service  <<EOF
[Unit]
Description=planq node service
After=network-online.target

[Service]
User=planq
ExecStart=${HOME}/bin/planqd start 
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
ln -sf ${HOME}/systemd/planqd-node.service /etc/systemd/system/
```

Reload Daemon

```bash
sudo systemctl daemon-reload
```

Enable Service when booting

```bash
sudo systemctl enable planqd-node.service
```

Start Service

```bash
sudo systemctl start planqd-node.service
```

Check Service

```bash
sudo systemctl status planqd-node.service
sudo journalctl -fu planqd-node.service
```
