---
description: Sudo For Application
cover: ../.gitbook/assets/Banner.jpg
coverY: 0
---

# 🟥 Sudo Management

```bash
salinem   ALL=(ALL) NOPASSWD:/usr/bin/systemctl * cosmovisor-comdex,/usr/bin/systemctl * gravityd,/usr/bin/journalctl -f -u *,/usr/bin/systemctl daemon-reload,/usr/bin/systemctl * gravity-orchestrator,/usr/bin/systemctl * gravity-relayer,/usr/bin/systemctl * ibc, /usr/bin/journalctl -fu *,/usr/bin/systemctl * nym-mixnode
```
