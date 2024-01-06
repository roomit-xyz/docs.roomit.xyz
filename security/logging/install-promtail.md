# Install Promtail

{% hint style="info" %}
Running as : root
{% endhint %}

Get and Set Version Promtail

```
PROMTAIL_VERSION=$(curl -s "https://api.github.com/repos/grafana/loki/releases/latest" | grep -Po '"tag_name": "v\K[0-9.]+')
```

Create FHS

```
mkdir -p /opt/promtail/{conf,bin,systemd}
```

Download and Install Promtail

```
wget -qO /opt/promtail/promtail.gz "https://github.com/grafana/loki/releases/download/v${PROMTAIL_VERSION}/promtail-linux-amd64.zip"
gunzip /opt/promtail/promtail.gz
chmod +x  /opt/promtail/promtail
mv /opt/promtail/promtail /opt/promtail/bin/
wget -qO /opt/promtail/conf/promtail.conf "https://raw.githubusercontent.com/grafana/loki/v${PROMTAIL_VERSION}/clients/cmd/promtail/promtail-local-config.yaml"
```

Systemd and Running

```bash
cat > /opt/promtail/systemd/promtail.service<<EOF
[Unit]
Description=Promtail client for sending logs to Loki
After=network.target

[Service]
ExecStart=/opt/promtail/bin/promtail -config.file=/opt/promtail/conf/promtail.conf
Restart=always
TimeoutStopSec=3

[Install]
WantedBy=multi-user.target
EOF

ln -sf /opt/promtail/systemd/promtail.service /etc/systemd/system/


systemctl daemon-reload
systemctl start promtail
```
