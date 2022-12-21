---
description: >-
  With our state sync services you will be able to catch up latest chain block
  in matter of minutes
---

# State Sync

{% hint style="info" %}
State Sync allows a new node to join the network by fetching a snapshot of the application state at a recent height instead of fetching and replaying all historical blocks. Since the application state is generally much smaller than the blocks, and restoring it is much faster than replaying blocks, this can reduce the time to sync with the network from days to minutes.
{% endhint %}

{% hint style="info" %}
**Running in user** : _salinem_
{% endhint %}

First Set Pruning Configuration, Set variable CONF\_VALIDATOR with .comdex refer [pruning-storage-tendermint.md](../../../automations/scripts/pruning-storage-tendermint.md "mention")



Stop and Reset Block Data

```bash
### STOP SERVICE
sudo systemctl stop cosmovisor-comdex

### BACKUP Private Validator
cp $HOME/.comdex/data/priv_validator_state.json $HOME/.comdex/priv_validator_state.json.backup

### RESET DATA
comdex tendermint unsafe-reset-all --home $HOME/.comdex


#### STATE VARIABLE
STATE_SYNC_RPC=https://rpc.comdex.roomit.xyz
STATE_SYNC_PEER=3d2e8b56deeb53a33397c9f03c0eb0c48f689e2@rpc.comdex.roomit.xyz
LATEST_HEIGHT=$(curl -s $STATE_SYNC_RPC/block | jq -r .result.block.header.height)
SYNC_BLOCK_HEIGHT=$(($LATEST_HEIGHT - 2000))
SYNC_BLOCK_HASH=$(curl -s "$STATE_SYNC_RPC/block?height=$SYNC_BLOCK_HEIGHT" | jq -r .result.block_id.hash)

sed -i.bak -e "s|^enable *=.*|enable = true|" $HOME/.comdex/config/config.toml
sed -i.bak -e "s|^rpc_servers *=.*|rpc_servers = \"$STATE_SYNC_RPC,$STATE_SYNC_RPC\"|" \
  $HOME/.comdex/config/config.toml
sed -i.bak -e "s|^trust_height *=.*|trust_height = $SYNC_BLOCK_HEIGHT|" \
  $HOME/.comdex/config/config.toml
sed -i.bak -e "s|^trust_hash *=.*|trust_hash = \"$SYNC_BLOCK_HASH\"|" \
  $HOME/.comdex/config/config.toml
sed -i.bak -e "s|^persistent_peers *=.*|persistent_peers = \"$STATE_SYNC_PEER\"|" \
  $HOME/.comdex/config/config.toml
mv $HOME/.comdex/priv_validator_state.json.backup $HOME/.comdex/data/priv_validator_state.json

```

Reset Config

```bash
comdex unsafe-reset-al
comdex tendermint unsafe-reset-all --home $HOME/.comdex --keep-addr-book
```

Download Wasm and Extract

```bash
wget -c https://roomit.xyz/download/wasm.tar.gz
tar xvfz wasm.tar.gz -C ${HOME}/.comdex/
rm wasm.tar.gz
```

Start Service

```bash
sudo systemctl start comovisor-comdex
```
