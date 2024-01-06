---
description: Relayer Documentation
---

# 🟢 IBC Relayer

{% hint style="info" %}
login to user : salinem
{% endhint %}

**Generate Keys For Gravity IBC | save as file \_ibc-gravity.json**\_

```bash
gravityd keys add ibc-gravity --keyring-backend file --output json     
```

**Generate Keys For Osmosis IBC | save as file \_ibc-osmosis.json**\_

{% hint style="info" %}
If you have not Node Osmosis, Download first binary
{% endhint %}

```bash
 wget -c https://github.com/osmosis-labs/osmosis/releases/download/v13.1.0/osmosisd-13.1.0-linux-amd64
 mv osmosisd-13.1.0-linux-amd64 osmosisd
```

Generate Keys

```bash
osmosisd keys add ibc-osmosis --keyring-backend file --output json 
```

**Generate Keys For Planq | Save as ibc-planq.json**

```bash
planqd keys add ibc-planq --keyring-backend file --output json 
```

**Install Hermes**

```bash
 wget -c https://github.com/informalsystems/ibc-rs/releases/download/v1.1.0/hermes-v1.1.0-x86_64-unknown-linux-gnu.tar.gzb
 tar xvf hermes-v1.1.0-x86_64-unknown-linux-gnu.tar.gz
 mkdir -p ibc-relayer/{bin,conf,systemd}
 mv hermes ibc-relayer/bin/
 mkdir ~/.hermes/{keys,wallets} 
```

**Copy all Keys json**

```
mv ibc-*.json ~/.hermes/wallets
```

**Create Config Hermes in \~/ibc-relayer/conf/config.toml**

```
# The global section has parameters that apply globally to the relayer operation.
[global]
log_level = 'info'

# Specify the mode to be used by the relayer. [Required]
[mode]

# Specify the client mode.
[mode.clients]
enabled = false
refresh = true
misbehaviour = false

# Specify the connections mode.
[mode.connections]
enabled = false

# Specify the channels mode.
[mode.channels]
enabled = false

# Specify the packets mode.
[mode.packets]
enabled = true
clear_interval = 200
clear_on_start = true
tx_confirmation = true


[rest]
enabled = true
host = '0.0.0.0'
port = 3001

[telemetry]
enabled = true
host = '0.0.0.0'
port = 4001

######## PLANQ #####
[[chains]]
id = 'planq_7070-2'
rpc_addr = 'xxxxxxx'
grpc_addr = 'xxxxxxx'
websocket_addr = 'xxxxxxxx'

rpc_timeout = '20s'
account_prefix = 'plq'
key_name = 'ibc-planq'
address_type = { derivation = 'ethermint', proto_type = { pk_type = '/ethermint.crypto.v1.ethsecp256k1.PubKey' } }
default_gas = 2000000
max_gas = 40000000
gas_multiplier = 1.3
store_prefix = 'ibc'
max_msg_num = 30
max_tx_size = 1800000
clock_drift = '15s'
max_block_time = '10s'
trusting_period = '7days'
memo_prefix = 'RoomIT_IBC'
trust_threshold = { numerator = '1', denominator = '3' }
gas_price = { price = 30000000000, denom = 'aplanq' }


[chains.packet_filter]
policy = 'allow'
list = [
  ['transfer', 'channel-0'], # Gravity
  ['transfer', 'channel-1'], # Osmosis
]



###### GRAVITY ####
[[chains]]
id = 'gravity-bridge-3'
rpc_addr = 'xxxxx'
grpc_addr = 'xxxxx'
websocket_addr = 'xxxxx'

rpc_timeout = '20s'
account_prefix = 'gravity'
key_name = 'xxxxx'
address_type = { derivation = 'cosmos' }
store_prefix = 'ibc'
default_gas = 300000
max_gas = 5000000
gas_price = { price = 0.000, denom = 'ugraviton' }
gas_multiplier = 1.4
max_msg_num = 30
max_tx_size = 1800000
clock_drift = '15s'
max_block_time = '10s'
trusting_period = '7days'
memo_prefix = 'RoomIT_IBC'
trust_threshold = { numerator = '1', denominator = '3' }

[chains.packet_filter]
policy = 'allow'
list = [
  ['transfer', 'channel-10'], # Osmosis
  'transfer', 'channel-102'], # Planq
]


##### OSMOSIS ###
[[chains]]
id = 'osmosis-1'
rpc_addr = 'xxxxxxx'
grpc_addr = 'xxxxxxxx'
websocket_addr = 'xxxxxxxxxxx'

rpc_timeout = '20s'
account_prefix = 'osmo'
key_name = 'ibc-osmosis'
address_type = { derivation = 'cosmos' }
store_prefix = 'ibc'
default_gas = 500000
max_gas = 120000000
gas_price = { price = 0.0026, denom = 'uosmo' }
gas_multiplier = 1.8
max_msg_num = 30
max_tx_size = 1800000
clock_drift = '15s'
max_block_time = '10s'
trusting_period = '7days'
memo_prefix = 'RoomIT_IBC'
trust_threshold = { numerator = '1', denominator = '3' }

[chains.packet_filter]
policy = 'allow'
list = [
   ['transfer', 'channel-144'], # gravity
   ['transfer', 'channel-492'], # planq
]

```

Assign as **xxxxx** with true value

**Copy All Config to \~/.hermes/**

```bash
cp  ~/ibc-relayer/conf/config.toml ~/.hermes/
```

**Add Keys to Hermes**

```bash
hermes keys add --chain gravity-bridge-3 --key-file .hermes/wallets/ibc-gravity.json
hermes keys add --chain osmosis-1 --key-file .hermes/ibc-osmosis.json
```

alternative

```
hermes keys add --chain planq_7070-2 --key-name ibc-planq --hd-path "m/44'/60'/0'/0/0" --mnemonic-file  ibc-planq.json
```

**Create Init Systemd**

```
cat > ~/ibc-relayer/ibc.service<EOF
[Unit]
Description=hermes ibc 
After=network-online.target

[Service]
User=tendermint
Group=tendermint
ExecStart=/mainnet/salinem/ibc-relayer/bin/hermes start 
Restart=on-failure
RestartSec=3
LimitNOFILE=500000
LimitNPROC=500000
Environment="HOME=/mainnet/salinem"


[Install]
WantedBy=multi-user.target
EOF
```

**Linking systemd**

```bash
sudo ln -sf /mainnet/salinem/ibc-relayer/systemd/ibc.service /etc/systemd/system
sudo systemctl daemon-reload
```

**Start Service Hermes**

```bash
sudo systemctl start ibc
```

**Create Connection**

```
  hermes create connection --a-chain planq_7070-2 --b-chain gravity-bridge-3
  hermes create connection --a-chain planq_7070-2  --b-chain osmosis-1
  hermes create connection --a-chain gravity-bridge-3  --b-chain osmosis-1
  hermes create connection --a-chain gravity-bridge-3  --b-chain planq_7070-2
  hermes create connection --a-chain osmosis-1  --b-chain planq_7070-2
  hermes create connection --a-chain osmosis-1  --b-chain gravity-bridge-3
```

**Test Transfer**

```
your_blockchain tx ibc-transfer transfer transfer channel-0 your_osmo_wallet_address 1000000usource --from your_wallet_name --gas-adjustment 1.4 --gas auto --gas-prices 0.025usource -y
```
