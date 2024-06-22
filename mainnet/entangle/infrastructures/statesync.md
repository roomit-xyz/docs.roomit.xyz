---
description: StateSync Entangle From RoomIT
---

Create Mini script, assume filename is statesync.sh

```bash
#!/bin/bash

SNAP_RPC="https://rpc.entangle.roomit.xyz:443"

LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 2000)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)

sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"|" $HOME/.entangled/config/config.toml
```


Stop Service
```bash
sudo systemctl stop entangle-node
```

>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop entangled__


Execute Script after Service Stopped
```bash
bash statesync.sh
```

Reset Node
```bash
# On some tendermint chains
entangled  unsafe-reset-all

# On other tendermint chains
entangled  tendermint unsafe-reset-all --home $HOME/.entangled --keep-addr-book
```

Start Services
```bash
sudo systemctl start entangle-node
```