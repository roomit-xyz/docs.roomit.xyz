---
description: StateSync Arkeo From RoomIT
---

Create Mini script, assume filename is statesync.sh

```bash
#!/bin/bash

SNAP_RPC="https://rpc.arkeo.roomit.xyz:443"

LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 2000)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)

sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"|" $HOME/.arkeo/config/config.toml
```


Stop Service
```bash
sudo systemctl stop arkeo-node
```

>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop arkeod__


Execute Script after Service Stopped
```bash
bash statesync.sh
```

Reset Node
```bash
# On some tendermint chains
arkeod  unsafe-reset-all

# On other tendermint chains
arkeod  tendermint unsafe-reset-all --home $HOME/.arkeo --keep-addr-book
```

Start Services
```bash
sudo systemctl start arkeo-node
```