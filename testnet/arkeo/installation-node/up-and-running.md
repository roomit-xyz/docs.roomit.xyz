---
description: How to start running Arkeo
---

# Up And Running

{%  hint style="info" %}
Ensure you have add sudoers to user salinem, refer [sudo-management.md](../../../security/sudo-management.md "mention")
{%  endhint %}

{%  hint style="info" %}
**Running in user** (Assume) : _salinem_ We never used this username in our production !
{%  endhint %}

#### Create Systemd

Create file in ${HOME}/systemd/arkeo-node.service

```bash
export HOME="/mainnet/salinem"
cat > ${HOME}/systemd/arkeo-node.service  <<EOF
[Unit]
Description=arkeo node service
After=network-online.target

[Service]
User=salinem
ExecStart=${HOME}/bin/arkeod start 
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
sudo cp ${HOME}/systemd/arkeo-node.service /etc/systemd/system/
```

Reload Daemon

```bash
sudo systemctl daemon-reload
```

Enable Service when booting

```bash
sudo systemctl enable arkeo-node.service
```

Start Service

```bash
sudo systemctl start arkeo-node.service
```

Check Service

```bash
sudo systemctl status arkeo-node.service
sudo journalctl -fu arkeo-node.service
```
