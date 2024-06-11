---
description: How to start running Selfchain
---

# Up And Running

{%  hint style="info" %}
Ensure you have add sudoers to user salinem, refer [sudo-management.md](../../../security/sudo-management.md "mention")
{%  endhint %}

{%  hint style="info" %}
**Running in user** (Assume) : _salinem_ We never used this username in our production !
{%  endhint %}

#### Create Systemd

Create file in ${HOME}/systemd/selfchain-node.service

```bash
export HOME="/mainnet/salinem"
cat > ${HOME}/systemd/selfchain-node.service  <<EOF
[Unit]
Description=selfchain node service
After=network-online.target

[Service]
User=salinem
ExecStart=${HOME}/bin/selfchaind start 
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
sudo cp ${HOME}/systemd/selfchain-node.service /etc/systemd/system/
```

Reload Daemon

```bash
sudo systemctl daemon-reload
```

Enable Service when booting

```bash
sudo systemctl enable selfchain-node.service
```

Start Service

```bash
sudo systemctl start selfchain-node.service
```

Check Service

```bash
sudo systemctl status selfchain-node.service
sudo journalctl -fu selfchain-node.service
```
