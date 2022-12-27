---
description: How to start running comdex
---

# Up And Running

{% hint style="info" %}
Ensure you have add sudoers, refer [sudo-management.md](../../../security/sudo-management.md "mention")
{% endhint %}

#### Create Exec Akash

````
```shellscript
cat <<EOF | tee ${HOME}/bin/akash-start.sh
#!/usr/bin/env bash

# prevents "panic: cannot delete latest saved version" error
export AKASH_PRUNING=nothing

# prevents "panic: runtime error: invalid memory address or nil pointer dereference" error
export AKASH_IAVL_DISABLE_FASTNODE=false

# prevents "panic: runtime error: invalid memory address or nil pointer dereference" error on cosmos-sdk's `createSnapshot ... incrVersionReaders`
export AKASH_STATESYNC_SNAPSHOT_INTERVAL=0

/mainnet/salinem/bin/akash start
EOF
```
````

#### Create Systemd

Create  file in ${HOME}/systemd/cosmovisor-comdex.service

```bash
cat > ${HOME}/systemd/akash-node.service << 'EOF'
[Unit]
Description=Akash Node
After=network.target

[Service]
User=akash
Group=akash
ExecStart=/mainnet/salinem/bin/akash-start.sh
KillSignal=SIGINT
Restart=on-failure
RestartSec=15
StartLimitInterval=200
StartLimitBurst=10
#LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

Linking to Systemd

```bash
ln -sf ${HOME}/systemd/akash-node.service /etc/systemd/system/
```

Reload Daemon

```bash
sudo systemctl daemon-reload
```

Enable Service when booting

```bash
sudo systemctl enable akash-node.service
```

Start Service

```bash
sudo systemctl start akash-node.service
```

Check Service

```bash
sudo systemctl status akash-node.service
sudo journalctl -fu akash-node.service
```
