---
description: How to start running Atomone
---

# Up And Running

{%  hint style="info" %}
Ensure you have add sudoers to user salinem, refer [sudo-management.md](../../../security/sudo-management.md "mention")
{%  endhint %}

{%  hint style="info" %}
**Running in user** (Assume) : _salinem_ We never used this username in our production !
{%  endhint %}

#### Create Systemd

Create file in ${HOME}/systemd/atomone-node.service

```bash
export HOME="/mainnet/salinem"
cat > ${HOME}/systemd/atomone-node.service  <<EOF
[Unit]
Description=atomone node service
After=network-online.target

[Service]
User=salinem
ExecStart=${HOME}/bin/atomoned start 
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
sudo cp ${HOME}/systemd/atomone-node.service /etc/systemd/system/
```

Reload Daemon

```bash
sudo systemctl daemon-reload
```

Enable Service when booting

```bash
sudo systemctl enable atomone-node.service
```

Start Service

```bash
sudo systemctl start atomone-node.service
```

Check Service

```bash
sudo systemctl status atomone-node.service
sudo journalctl -fu atomone-node.service
```
