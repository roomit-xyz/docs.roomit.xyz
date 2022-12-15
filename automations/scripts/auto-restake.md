---
description: This Script for auto restake own wallet
---

# Auto Restake

{% hint style="info" %}
For Used this script in production. Please encrypt this script, due password attached.

We recommend for compile this script with shc. Refer : [compile-script-bash.md](../../security/other/compile-script-bash.md "mention")
{% endhint %}

```bash
#!/bin/bash
#
# RoomIT
# https://roomit.xyz
# If this script useful and you will visit cikarang indonesia, 
# Let's drink coffee and talk about blockchain
#
#

set -e
HOME_VALIDATOR=""
DELEGATOR_ADDRESS=""
VALIDATOR_ADDRESS=""
VALIDATOR_CONF=""
VALIDATOR_RPC=""
CHAIN_ID=""
KEY_NAME=""
UNIT_COIN=""
AMOUNT=""
BIN=""
HOLD=
PASS=""


CHECK_REWARDS=`${BIN} q distribution rewards ${DELEGATOR_ADDRESS} ${VALIDATOR_ADDRESS} -o json --home ${HOME_VALIDATOR}/${VALIDATOR_CONF} --node ${VALIDATOR_RPC} | jq -r '.rewards[] | select(.denom=="ugraviton") | .amount'`
if [ `printf "%.0f \n" ${CHECK_REWARDS}` -gt ${HOLD} ]
then
        echo "${PASS}" | ${BIN} tx distribution withdraw-all-rewards --from=${KEY_NAME} --chain-id=${CHAIN_ID}  --gas-adjustment=1.4 --gas=auto  --home ${HOME_VALIDATOR}/${VALIDATOR_CONF}  --node ${VALIDATOR_RPC}  -y
	sleep 30;
else
	echo "`date` Rewards < $((HOLD / 1000000)) ${UNIT_COIN:1} "
fi

CHECK_COMMISSION=`${BIN}  q distribution validator-outstanding-rewards ${VALIDATOR_ADDRESS} -o json --home ${HOME_VALIDATOR}/${VALIDATOR_CONF} --node ${VALIDATOR_RPC} | jq -r '.rewards[] | select(.denom=="ugraviton") | .amount'`
if [ `printf "%.0f \n" ${CHECK_COMMISSION}` -gt ${HOLD} ]
then
   echo "${PASS}" | ${BIN} tx distribution withdraw-rewards ${VALIDATOR_ADDRESS} --from=${KEY_NAME} --commission --chain-id=${CHAIN_ID} --gas-adjustment=1.4 --gas=auto  --home ${HOME_VALIDATOR}/${VALIDATOR_CONF} --node ${VALIDATOR_RPC} -y
   sleep 30;
else
   echo "`date` Commision Reward <  $((HOLD / 1000000)) ${UNIT_COIN:1}"
fi


CHECK_BALANCES=$(${BIN} q bank balances ${DELEGATOR_ADDRESS} --home ${HOME_VALIDATOR}/${VALIDATOR_CONF} --node ${VALIDATOR_RPC}  --output json |  jq -r ".balances[] | select(.denom == \"${UNIT_COIN}\") | .amount" )
if [ `printf "%.0f \n" ${CHECK_BALANCES}` -gt ${HOLD} ]
then
   WILL_DELEGATE_BALANCES=$((${CHECK_BALANCES}-${HOLD}))
   if [ ${WILL_DELEGATE_BALANCES} -gt ${HOLD} ]
   then
      echo "${PASS}" | ${BIN} tx staking delegate ${VALIDATOR_ADDRESS} ${WILL_DELEGATE_BALANCES}${UNIT_COIN} --from=${KEY_NAME} --chain-id=${CHAIN_ID}  --gas-adjustment=1.4 --gas=auto --node ${VALIDATOR_RPC} --home ${HOME_VALIDATOR}/${VALIDATOR_CONF} -y
   else
      echo "`date` Balances for staking <   $((HOLD / 1000000)) ${UNIT_COIN:1}"
   fi
else
   echo "`date` There is no Delegation"
fi

```
