---
description: How to start running comdex
---

# Up And Running

{% hint style="info" %}
Ensure you have add sudoers, refer [sudo-management.md](../../../../security/sudo-management.md "mention")
{% endhint %}

#### Create Systemd

Create  file in ${HOME}/systemd/cosmovisor-comdex.service

```bash
[Unit]
Description=cosmovisor comdex
After=network-online.target

[Service]
User=comdex
ExecStart=/app/comdex/bin/cosmovisor start
Restart=always
RestartSec=3
LimitNOFILE=4096
Environment="DAEMON_NAME=comdex"
Environment="DAEMON_HOME=/app/comdex/.comdex"
Environment="DAEMON_ALLOW_DOWNLOAD_BINARIES=false"
Environment="DAEMON_RESTART_AFTER_UPGRADE=true"
Environment="DAEMON_LOG_BUFFER_SIZE=512"

[Install]
WantedBy=multi-user.target
```

Linking to Systemd

```bash
ln -sf ${HOME}/systemd/cosmovisor-comdex.service /etc/systemd/system/
```

Reload Daemon

```bash
sudo systemctl daemon-reload
```

Enable Service when booting

```bash
sudo systemctl enable cosmovisor-comdex
```

Start Service

```bash
sudo systemctl start cosmovisor-comdex
```

Check Service

```bash
sudo systemctl status cosmovisor-comdex
sudo journalctl -fu cosmovisor-comdex
```
