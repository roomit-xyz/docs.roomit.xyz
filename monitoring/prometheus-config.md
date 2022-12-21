# 🟪 Prometheus Config

```
## PRODUCTION APPLICATIONS METRICS
  - job_name: "Production"
    metrics_path: '/metrics'
    static_configs:
     - targets: ["mainnet-02.roomit.xyz:9100","mainnet-01.roomit.xyz:9100","system.roomit.xyz:9100"]
       labels:
         group: 'resource'
     - targets: ["mainnet-01.roomit.xyz:9101"]
       labels:
         group: 'nym'
     - targets: ["mainnet-01.roomit.xyz:9102"]
       labels:
         group: 'myst'
     - targets: ["mainnet-02.roomit.xyz:16800"]
       labels:
         group: 'gravity-node'
     - targets: ["mainnet-02.roomit.xyz:16801"]
       labels:
         group: 'comdex-node'
     - targets: ["mainnet-02.roomit.xyz:1500"]
       labels:
         group: 'gravity-orchestrator'
```
