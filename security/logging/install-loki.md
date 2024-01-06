---
description: Installation Loki
---

# Install Loki

{% hint style="info" %}
Running with user : root
{% endhint %}

Get and Set Version Loki Latest

```
LOKI_VERSION=$(curl -s "https://api.github.com/repos/grafana/loki/releases/latest" | grep -Po '"tag_name": "v\K[0-9.]+')
```

Create FHS

```bash
mkdir -p /opt/loki/{bin,systemd}
```

Setting Shell Variable

<pre class="language-bash"><code class="lang-bash"><strong>cat > /etc/profile.d/loki.sh &#x3C;&#x3C;EOF
</strong><strong>#!/bin/bash
</strong>export PATH="${PATH}:/opt/loki/bin"
EOF
</code></pre>

Download Loki

```bash
wget -qO /opt/loki/loki.gz "https://github.com/grafana/loki/releases/download/v${LOKI_VERSION}/loki-linux-amd64.zip"
gunzip /opt/loki/loki.gz
mv loki /opt/loki/loki /opt/loki/bin/loki
rm  /opt/loki/loki.gz
```

Reload Shell

```bash
source /etc/profile.d/loki.sh
```

Download Config Loki

```bash
wget -qO /opt/loki/conf/loki.conf "https://raw.githubusercontent.com/grafana/loki/v${LOKI_VERSION}/cmd/loki/loki-local-config.yaml"
```

Verify Loki

```
loki -version
```

Create Systemd Init

```bash
cat > /opt/loki/systemd/loki.service<<EOF
[Unit]
Description=Loki log aggregation system
After=network.target

[Service]
ExecStart=/opt/loki/bin/loki -config.file=/opt/loki/conf/loki.conf
Restart=always

[Install]
WantedBy=multi-user.target
EOF
```

Linking systemd

```bash
ln -sf /opt/loki/systemd/loki.service /etc/systemd/system/
```

Reload and Start Service

```bash
systemctl daemon-reload
systemctl start loki
```
