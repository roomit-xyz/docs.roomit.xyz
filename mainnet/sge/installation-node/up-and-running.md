---
description: How to start running SGE
---

# Up And Running

{% hint style="info" %}
Ensure you have add sudoers, refer [sudo-management.md](../../../security/sudo-management.md "mention")
{% endhint %}

{% hint style="info" %}
**Running in user** (Assume) : _salinem_ We never used this username in our production !
{% endhint %}

#### Create Systemd

Create file in ${HOME}/systemd/sge-node.service

```bash
export HOME="/mainnet/salinem"
cat > ${HOME}/systemd/sge-node.service  <<EOF
[Unit]
Description=SGE node service
After=network-online.target

[Service]
User=sge
ExecStart=${HOME}/bin/sged start 
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
ln -sf ${HOME}/systemd/sge-node.service /etc/systemd/system/
```

Reload Daemon

```bash
sudo systemctl daemon-reload
```

Enable Service when booting

```bash
sudo systemctl enable sge-node.service
```

Start Service

```bash
sudo systemctl start sge-node.service
```

Check Service

```bash
sudo systemctl status sge-node.service
sudo journalctl -fu sge-node.service
```
