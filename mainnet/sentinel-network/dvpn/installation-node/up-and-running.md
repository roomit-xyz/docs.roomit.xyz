---
description: How to start running dVPN Sentinel
---

# Up And Running

{% hint style="info" %}
Ensure you have add sudoers, refer [sudo-management.md](../../../../security/sudo-management.md "mention")
{% endhint %}

{% hint style="info" %}
**Running in user** (Assume) : _salinem_ We never used this username in our production !
{% endhint %}

#### Create Systemd

Create file in ${HOME}/system/dvpn-node.service

```bash
export HOME="/mainnet/salinem"
cat > ${HOME}/systemd/dvpn-node.service  <<EOF
[Unit]
Description=dvpn node service
After=network-online.target

[Service]
User=salinem
ExecStart=${HOME}/bin/sentinelhub start 
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
ln -sf ${HOME}/systemd/dvpn-node.service /etc/systemd/system/
```

Reload Daemon

```bash
sudo systemctl daemon-reload
```

Enable Service when booting

```bash
sudo systemctl enable dvpn-node.service
```

Start Service

```bash
sudo systemctl start dvpn-node.service
```

Check Service

```bash
sudo systemctl status dvpn-node.service
sudo journalctl -fu dvpn-node.service
```
