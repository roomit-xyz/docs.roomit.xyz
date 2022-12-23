---
description: Check balance address gravity
---

# Check Balance Address Gravity

{% hint style="info" %}
Ensure you have done install telegram-send with python3
{% endhint %}

```bash
#!/bin/bash
#
# RoomIT
# https://roomit.xyz
# If this script useful and you will visit cikarang indonesia, 
# Let's drink coffee and talk about blockchain
#



URL="https://api.gravity.roomit.xyz/cosmos/bank/v1beta1/balances"

### GRAVITY
WALLET_GRAVITY_NODE="gravity1ssduj8c0cc8kquljvw3ygq9hduvcysnf9wkp3k,node"
WALLET_GRAVITY_ORCHESTRATOR="gravity187dgvpsaeknguy8sksewldqcp9wukvy7f6zg6t,orchestrator"
WALLET_GRAVITY_IBC_RELAYER="gravity1r0awpz4zkrv5revdyssv6c0janxzqfjr470zzy,ibc"
WALLET_GRAVITY=("${WALLET_GRAVITY_NODE}" "${WALLET_GRAVITY_ORCHESTRATOR}" "${WALLET_GRAVITY_IBC_RELAYER}")




for i in ${WALLET_GRAVITY[@]}
do
    KIND_WALLET=`echo $i | awk -F"," '{print $2}'`
    ADDRESS_WALLET=`echo $i | awk -F"," '{print $1}'`
    BALANCE_WALLET=`curl  ${URL}/${ADDRESS_WALLET}| jq -r ".balances | .[]|.amount"`
    HUMAN_BALANCED=`echo "$BALANCE_WALLET/1000000" | bc -l |sed 's/^\./0./'`
    telegram-send "${KIND_WALLET} | ${ADDRESS_WALLET} | `printf  '%0.3f\n' ${HUMAN_BALANCED}`" 
done

```

