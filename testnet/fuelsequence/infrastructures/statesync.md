---
description: StateSync Fuelsequence From RoomIT
---

Create Mini script, assume filename is statesync.sh

```bash
#!/bin/bash

SNAP_RPC="https://rpc.fuelsequence.roomit.xyz:443"

LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 2000)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)

sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"|" $HOME/.fuelsequencer/config/config.toml
```


Stop Service
```bash
sudo systemctl stop fuelsequence-node
```

>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop fuelsequencerd__


Execute Script after Service Stopped
```bash
bash statesync.sh
```

Reset Node
```bash
# On some tendermint chains
fuelsequencerd  unsafe-reset-all

# On other tendermint chains
fuelsequencerd  tendermint unsafe-reset-all --home $HOME/.fuelsequencer --keep-addr-book
```

Start Services
```bash
sudo systemctl start fuelsequence-node
```