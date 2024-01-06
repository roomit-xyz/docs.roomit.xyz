---
description: How to start running gitopia
---

# Up And Running

{% hint style="info" %}
**Running in user** (Assume) : _salinem_ We never used this username in our production !
{% endhint %}

{% hint style="info" %}
Ensure you have add sudoers, refer [sudo-management.md](../../../security/sudo-management.md "mention")
{% endhint %}

#### Create Systemd

Create file in ${HOME}/systemd/gitopia-node.service

```bash
export HOME="/mainnet/salinem"
cat > ${HOME}/systemd/gitopia-node.service  <<EOF
[Unit]
Description=Gitopia node service
After=network-online.target

[Service]
User=gitopia
ExecStart=${HOME}/bin/gitopiad start 
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
ln -sf ${HOME}/systemd/gitopia-node.service /etc/systemd/system/
```

Reload Daemon

```bash
sudo systemctl daemon-reload
```

Enable Service when booting

```bash
sudo systemctl enable gitopia-node.service
```

Start Service

```bash
sudo systemctl start gitopia-node.service
```

Check Service

```bash
sudo systemctl status gitopia-node.service
sudo journalctl -fu gitopia-node.service
```
