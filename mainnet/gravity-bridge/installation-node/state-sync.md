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

Ensure you have add sudoers, refer [sudo-management.md](../../../security/sudo-management.md "mention")
{% endhint %}

Stop and Reset Block Data

```bash
### STOP SERVICE
sudo systemctl stop gravityd

### BACKUP Private Validator
cp $HOME/.gravity/data/priv_validator_state.json $HOME/.gravity/priv_validator_state.json.backup

### RESET DATA
gravityd tendermint unsafe-reset-all --home $HOME/.gravity


#### STATE VARIABLE
STATE_SYNC_RPC=https://rpc.gravity.roomit.xyz
STATE_SYNC_PEER=db27f775fda5ac99150a78a636a8bb857247327f@rpc.gravity.roomit.xyz
LATEST_HEIGHT=$(curl -s $STATE_SYNC_RPC/block | jq -r .result.block.header.height)
SYNC_BLOCK_HEIGHT=$(($LATEST_HEIGHT - 2000))
SYNC_BLOCK_HASH=$(curl -s "$STATE_SYNC_RPC/block?height=$SYNC_BLOCK_HEIGHT" | jq -r .result.block_id.hash)

sed -i.bak -e "s|^enable *=.*|enable = true|" $HOME/.gravity/config/config.toml
sed -i.bak -e "s|^rpc_servers *=.*|rpc_servers = \"$STATE_SYNC_RPC,$STATE_SYNC_RPC\"|" \
  $HOME/.gravity/config/config.toml
sed -i.bak -e "s|^trust_height *=.*|trust_height = $SYNC_BLOCK_HEIGHT|" \
  $HOME/.gravity/config/config.toml
sed -i.bak -e "s|^trust_hash *=.*|trust_hash = \"$SYNC_BLOCK_HASH\"|" \
  $HOME/.gravity/config/config.toml
sed -i.bak -e "s|^persistent_peers *=.*|persistent_peers = \"$STATE_SYNC_PEER\"|" \
  $HOME/.gravity/config/config.toml
mv $HOME/.gravity/priv_validator_state.json.backup $HOME/.gravity/data/priv_validator_state.json
```

Restart Service

```bash
sudo systemctl start gravityd && journalctl -u gravityd -f 
```
