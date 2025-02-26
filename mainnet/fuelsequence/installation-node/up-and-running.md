---
description: How to start running Fuelsequence
---

# Up And Running

{%  hint style="info" %}
Ensure you have add sudoers to user salinem, refer [sudo-management.md](../../../security/sudo-management.md "mention")
{%  endhint %}

{%  hint style="info" %}
**Running in user** (Assume) : _salinem_ We never used this username in our production !
{%  endhint %}

#### Create Systemd

Create file in ${HOME}/systemd/fuelsequence-node.service

```bash
export HOME="/mainnet/salinem"
cat > ${HOME}/systemd/fuelsequence-node.service  <<EOF
[Unit]
Description=fuelsequence node service
After=network-online.target

[Service]
User=salinem
ExecStart=${HOME}/bin/fuelsequenced start 
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
sudo cp ${HOME}/systemd/fuelsequence-node.service /etc/systemd/system/
```

Reload Daemon

```bash
sudo systemctl daemon-reload
```

Enable Service when booting

```bash
sudo systemctl enable fuelsequence-node.service
```

Start Service

```bash
sudo systemctl start fuelsequence-node.service
```

Check Service

```bash
sudo systemctl status fuelsequence-node.service
sudo journalctl -fu fuelsequence-node.service
```
