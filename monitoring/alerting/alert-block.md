---
description: Alert Block if Many Missed Block
---

# Alerting Missed Block

```bash
#!/bin/bash
#
# RoomIT
# https://roomit.xyz
# If this script useful and you will visit cikarang indonesia, 
# Let's drink coffee and talk about blockchain
#
#
RPC_TENDERMINT="https://gravitychain.io:26657"
RPC_ROOMIT="https://rpc.gravity.roomit.xyz"

#### URL
HOST="https://health.roomit.xyz"
TOKEN=""

####### LOGIC
# (tendermint_consensus_height{group="gravity-node"} - 1) - tendermint_consensus_latest_block_height{group="gravity-node"}

CHECK_BLOCK_LAST_TENDERMINT=`curl -s ${RPC_TENDERMINT}/status | jq -r .result.sync_info.latest_block_height`
CHECK_BLOCK_CONSENSUS_ROOMIT=`curl -s ${RPC_ROOMIT}/consensus_params | jq -r .result.block_height`


RESULT=`echo "(${CHECK_BLOCK_CONSENSUS_ROOMIT} - 1) - ${CHECK_BLOCK_LAST_TENDERMINT} " |bc`
echo $RESULT

if [ ${RESULT} -le 2 ]
then
   MESSAGE="Block-OK"
   curl -s "${HOST}/api/push/${TOKEN}?status=up&msg=${MESSAGE}&ping="
else
   MESSAGE="Block-NOK"
   curl -s "${HOST}/api/push/${TOKEN}?status=down&msg=${MESSAGE}&ping="
fi

```
