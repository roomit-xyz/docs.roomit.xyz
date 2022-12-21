# Alerting Jailing

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


CHECK_JQ=`command -v jq`
if [ $? -eq 1 ]
then
   echo "Install bc | apt install jq or yum install bjq |"
   exit
fi


function URL(){
   URL=$(echo "${MESSAGE}" | sed -e 's/%/%25/g' -e 's/ /%20/g' -e 's/!/%21/g' -e 's/"/%22/g' -e 's/#/%23/g' -e 's/\$/%24/g' -e 's/\&/%26/g' -e 's/'\''/%27/g' -e 's/(/%28/g' -e 's/)/%29/g' -e 's/\*/%2a/g' -e 's/+/%2b/g' -e 's/,/%2c/g' -e 's/-/%2d/g' -e 's/\./%2e/g' -e 's/\//%2f/g' -e 's/:/%3a/g' -e 's/;/%3b/g' -e 's//%3e/g' -e 's/?/%3f/g' -e 's/@/%40/g' -e 's/\[/%5b/g' -e 's/\\/%5c/g' -e 's/\]/%5d/g' -e 's/\^/%5e/g' -e 's/_/%5f/g' -e 's/`/%60/g' -e 's/{/%7b/g' -e 's/|/%7c/g' -e 's/}/%7d/g' -e 's/~/%7e/g')
}



GET_STATUS=`${BIN} query staking validator ${VALIDATOR_ADDRESS} --chain-id ${CHAIN_ID} --node ${VALIDATOR_RPC} --output json | jq -r .jailed`
if [ "${GET_STATUS}" == "false" ] 
then
   MESSAGE="Node Not Jailed"
   URL;
   curl -s "${HOST}/api/push/${TOKEN}?status=up&msg=${URL}&ping="
else
   MESSAGE="Node Jailed"
   URL;
   curl -s "${HOST}/api/push/${TOKEN}?status=down&msg=${URL}&ping="
fi

```
