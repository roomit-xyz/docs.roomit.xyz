---
description: Checking Periodic Active or Inactive Node
---

# Alert Status Active or Inactive

```bash
#!/bin/bash
#
# RoomIT
# https://roomit.xyz
# If this script useful and you will visit cikarang indonesia,
# Let's drink coffee and talk about blockchain
#

set -e
HOME_VALIDATOR=""
DELEGATOR_ADDRESS=''
VALIDATOR_ADDRESS=''
VALIDATOR_CONF=""
VALIDATOR_RPC=""
CHAIN_ID=""
KEY_NAME=""
UNIT_COIN=""
AMOUNT=""
BIN=""

#### URL
HOST="https://health.roomit.xyz"
TOKEN=""

GET_STATUS=`${BIN} query staking validator ${VALIDATOR_ADDRESS} --chain-id ${CHAIN_ID} --node ${VALIDATOR_RPC} --output json | jq -r .status`
if [ "${GET_STATUS}" == "BOND_STATUS_BONDED" ] 
then
   MESSAGE="Active"
   curl -s "${HOST}/api/push/${TOKEN}?status=up&msg=${MESSAGE}&ping=50"
else
   MESSAGE="Inactive"
   curl -s "${HOST}/api/push/${TOKEN}?status=down&msg=${MESSAGE}&ping=50"
fi

```
