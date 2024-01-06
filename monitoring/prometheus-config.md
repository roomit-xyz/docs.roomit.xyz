---
cover: ../.gitbook/assets/Banner.jpg
coverY: 0
---

# 🟪 Prometheus Config

```
# my global config
global:
  scrape_interval: 1m # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  scrape_timeout: 1m
  evaluation_interval: 30s # Evaluate rules every 15 seconds. The default is every 1 minute.

# Alertmanager configuration
alerting:
  alertmanagers:
    - static_configs:
        - targets:
          # - alertmanager:9093

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  # - "first_rules.yml"
  # - "second_rules.yml"

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
## PRODUCTION APPLICATIONS METRICS
  ### UPTIME KUMA
  - job_name: 'uptime'
    scrape_interval: 30s
    scheme: https
    static_configs:
      - targets: ['health.roomit.xyz']
    basic_auth: 
      password: xxxxxxxxxxxxx
  
  ### TELEGRAF SYSTEM OS
  - job_name: "Hosts"
    metrics_path: '/metrics'
    static_configs:
      - targets: ["mainnet-02.roomit.xyz:9100","system.roomit.xyz:9100","horcrux-01.roomit.xyz:9100","horcrux-02.roomit.xyz:9100","horcrux-03.roomit.xyz:9100"]

  ### TENDERMINT METRICS
  - job_name: "Production"
    metrics_path: '/metrics'
    static_configs:
     - targets: ["mainnet-02.roomit.xyz:16800"]
       labels:
          instance: gravity-bridge 
     - targets: ["mainnet-02.roomit.xyz:16801"]
       labels:
          instance: gitopia
     - targets: ["mainnet-02.roomit.xyz:16802"]
       labels:
          instance: 8ball
     - targets: ["mainnet-02.roomit.xyz:16803"]
       labels:
          instance: planq
  
  ### HERMES METRICS         
  - job_name: "hermes"
    metrics_path: '/metrics'
    static_configs:
    - targets: ["mainnet-02.roomit.xyz:4001"]

  ### VALIDATOR PLANQ
  - job_name: 'validator-planq'
    scrape_interval: 15s
    metrics_path: /metrics/validator
    static_configs:
      - targets:
        - plqvaloper1fqnr328nlndkxek2jaz8teec0euyr5yh26q26l
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_address
      - source_labels: [__param_address]
        target_label: instance
      - target_label: __address__
        replacement: mainnet-02.roomit.xyz:1603
  # specific wallet(s)
  - job_name: 'wallet-planq'
    scrape_interval: 15s
    metrics_path: /metrics/wallet
    static_configs:
      - targets:
        - plq1fqnr328nlndkxek2jaz8teec0euyr5yh5ydsuw
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_address
      - source_labels: [__param_address]
        target_label: instance
      - target_label: __address__
        replacement: mainnet-02.roomit.xyz:1603

  ### VALIDATORS GITOPIA
  - job_name: 'validator-gitopia'
    scrape_interval: 15s
    metrics_path: /metrics/validator
    static_configs:
      - targets:
        - gitopiavaloper1pv8fkl4t7wk9mwptkwf8pemy9rt8qpkydr6k3p
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_address
      - source_labels: [__param_address]
        target_label: instance
      - target_label: __address__
        replacement: mainnet-02.roomit.xyz:1601
  # specific wallet(s)
  - job_name: 'wallet-gitopia'
    scrape_interval: 15s
    metrics_path: /metrics/wallet
    static_configs:
      - targets:
        - gitopia1pv8fkl4t7wk9mwptkwf8pemy9rt8qpky9e7x36
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_address
      - source_labels: [__param_address]
        target_label: instance
      - target_label: __address__
        replacement: mainnet-02.roomit.xyz:1601
    # all validators
  - job_name: 'validators-all-gitopia'
    scrape_interval: 15s
    metrics_path: /metrics/validators
    static_configs:
      - targets:
        - mainnet-02.roomit.xyz:1601


  ### HORCRUX
  - job_name: 'horcrux'
    scrape_interval: 15s
    metrics_path: /metrics
    static_configs:
    - targets: ["horcrux-01.roomit.xyz:1601"]
      labels: {"chain":"gitopia", "share":"1"}
    - targets: ["horcrux-02.roomit.xyz:1601"]
      labels: {"chain":"gitopia", "share":"2"}
    - targets: ["horcrux-03.roomit.xyz:1601"]
      labels: {"chain":"gitopia", "share":"3"}
    - targets: ["horcrux-01.roomit.xyz:1603"]
      labels: {"chain":"planq_7070-2", "share":"1"}
    - targets: ["horcrux-02.roomit.xyz:1603"]
      labels: {"chain":"planq_7070-2", "share":"2"}
    - targets: ["horcrux-03.roomit.xyz:1603"]
      labels: {"chain":"planq_7070-2", "share":"3"}

```
