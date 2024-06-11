---
description: How to start running Entangle
---

# Up And Running

{%  hint style="info" %}
Ensure you have add sudoers to user salinem, refer [sudo-management.md](../../../security/sudo-management.md "mention")
{%  endhint %}

{%  hint style="info" %}
**Running in user** (Assume) : _salinem_ We never used this username in our production !
{%  endhint %}

#### Create Systemd

Create file in ${HOME}/systemd/entangle-node.service

```bash
export HOME="/mainnet/salinem"
cat > ${HOME}/systemd/entangle-node.service  <<EOF
[Unit]
Description=entangle node service
After=network-online.target

[Service]
User=salinem
ExecStart=${HOME}/bin/entangled start 
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
sudo cp ${HOME}/systemd/entangle-node.service /etc/systemd/system/
```

Reload Daemon

```bash
sudo systemctl daemon-reload
```

Enable Service when booting

```bash
sudo systemctl enable entangle-node.service
```

Start Service

```bash
sudo systemctl start entangle-node.service
```

Check Service

```bash
sudo systemctl status entangle-node.service
sudo journalctl -fu entangle-node.service
```
