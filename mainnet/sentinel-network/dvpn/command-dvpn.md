---
description: This is command about CLI dVPN Sentinel Network
---

# Command Sentinel

### **Key Management**

{% hint style="info" %}
we assume, variable name\_your\_wallet is name of your own wallet.

Install jq package for management json format

**Ubuntu**

`apt install jq`

**`CentOS`**

`yum install jq`

**Arch Linux**

`pacman -S jq`
{% endhint %}

{% hint style="info" %}
**Running in user** (Assume) : _salinem_

We never use this username in our production!
{% endhint %}

#### **Add New Key**

```bash
sentinelhub keys add mywallet --home ${HOME}/.sentinelhub    
```

#### **Recover Key**

With Passphrase

```bash
sentinelhub keys add mywallet --recover  --home ${HOME}/.sentinelhub          
```

With Keyring

```bash
sentinelhub keys add mywallet --recover --keyring-backend os --home ${HOME}/.sentinelhub          
```

#### **List Key**

```bash
sentinelhub keys list --home ${HOME}/.sentinelhub       
```

#### **Delete Key**

```bash
sentinelhub keys delete mywallet --home ${HOME}/.sentinelhub
```

**Export Key**

```bash
 sentinelhub keys export mywallet --home ${HOME}/.sentinelhub
```

#### Import Key

```
sentinelhub keys import mywallet mywallet_file.backup --home ${HOME}/.sentinelhub
```

#### Show All Balances Address

<pre class="language-bash"><code class="lang-bash"><strong>for mywallet in `sentinelhub keys list --home ${HOME}/.sentinelhub--output json| jq -r ".[] .address"`
</strong>do
   CHAIN_ID="sentinelhub-2"
   RPC="tcp://localhost:16704"
   sentinelhub q bank balances ${mywallet} --home ${HOME}/.sentinelhub--chain-id ${CHAIN_ID} --node ${RPC}
done
</code></pre>

#### Show Balance Address

```bash
sentinelhub q bank balances mywallet_public_address --home ${HOME}/.sentinelhub--chain-id sentinelhub-2 --node tcp://localhost:16704
```

### Validator Management

{% hint style="info" %}
We assume,You have complete identities. Moniker, Website, Security and Details Your Validator

Install jq package for management json format

**Ubuntu**

`apt install jq`

**`CentOS`**

`yum install jq`

**Arch Linux**

`pacman -S jq`
{% endhint %}

#### Create New Validator

```bash
MONIKER="NAME_OF_YOUR_VALIDATOR"
PROFILE="PGP_KEY_OF_KEYBASE"
DETAILS="Describes Your Validator"
WEBSITE="https://yourwebsite.com"

sentinelhub tx staking create-validator \
--amount=1000000udvpn \
--pubkey=$(sentinelhub tendermint show-validator --home ${HOME}/.sentinelhub) \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}" \
--chain-id=sentinelhub-2 \
--commission-rate=0.05 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1 \
--from=mywallet \
--gas-adjustment=1.4 \
--gas=auto \
--node tcp://localhost:16704 \
--home ${HOME}/.sentinelhub\
-y
```

#### Edit Validator Identities Informations

```bash
MONIKER="NAME_OF_YOUR_VALIDATOR"
PROFILE="PGP_KEY_OF_KEYBASE"
DETAILS="Describes Your Validator"
WEBSITE="https://yourwebsite.com"

sentinelhub tx staking edit-validator \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}" \
--chain-id=sentinelhub-2 \
--commission-rate=0.05 \
--from=mywallet \
--gas-adjustment=1.4 \
--gas=auto \
--node tcp://localhost:16704 \
--home ${HOME}/.sentinelhub\
-y
```

#### Get Validator Info

```
sentinelhub status 2>&1 | jq .ValidatorInfo
```

#### Get Syncing Block

```
sentinelhub status 2>&1 | jq .SyncInfo
```

#### Get Peer Own Node

```bash
echo $(sentinelhub tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.sentinelhub/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

#### Get Peer Node

```
curl -sS http://localhost:16704/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

#### Unjail Validator

```bash
sentinelhub tx slashing unjail \
--from mywallet \
--chain-id sentinelhub-2 \
 --home ${HOME}/.sentinelhub\
 --node  tcp://localhost:16704 \
 --gas auto \
 --gas-adjustment 1.4 \
 -y
```

#### Jail Reason

```bash
sentinelhub query slashing signing-info $(sentinelhub tendermint show-validator) \
 --node  tcp://localhost:16704 \
 --home ${HOME}/.sentinelhub
```

#### List All Active Validator

```bash
sentinelhub q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### List All Inactive Validator

```bash
sentinelhub q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### Show Validator Details

```
sentinelhub \
query staking validator \
$(sentinelhub keys show \ 
                $(sentinelhub keys list --home ${HOME}/.sentinelhub--output json| jq -r ".[] .address" | tail -n1) \
--bech val -a) \
--chain-id sentinelhub-2 \
--node tcp://localhost:16704
```

### Token Management

#### Withdraw All Reward From Validator

```bash
sentinelhub tx distribution withdraw-all-rewards \
--from mywallet \
--chain-id sentinelhub-2 \
--node tcp://localhost:16704 \
--home ${HOME}/.sentinelhub\
--gas-adjustment 1.4 \
--gas auto \
-y
```

#### Withdraw Commision Reward From Validator

```bash
sentinelhub tx distribution withdraw-rewards $(sentinelhub keys show mywallet --bech val -a) \
--commission \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--chain-id sentinelhub-2 \
--node tcp://localhost:16704 \
--home ${HOME}/.sentinelhub\
-y 
```

#### Delegate My Token to Own Validator

```bash
sentinelhub tx staking delegate $(sentinelhub keys show wallet --bech val -a) 1000000udvpn \
--from mywallet \
--gas-adjustment 1.4 \
--home ${HOME}/.sentinelhub\
--node tcp://localhost:16704 \
--chain-id sentinelhub-2 \
--gas auto \
-y
```

#### Delegate Your Token To Our Validator

```bash
sentinelhub tx staking delegate gravityvaloper1ssduj8c0cc8kquljvw3ygq9hduvcysnf590lmz 1000000udvpn \ 
--from mywallet \
--gas-adjustment 1.4 \ 
--gas auto \
--home ${HOME}/.sentinelhub\
--node tcp://localhost:16704 \
--chain-id sentinelhub-2 \
-y
```

#### Redelegate Tokens to Another Validator

```bash
sentinelhub tx staking redelegate $(sentinelhub keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000udvpn \
--from mywallet 
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.sentinelhub\
--node tcp://localhost:16704 \
--chain-id sentinelhub-2 \
-y 
```

#### Unbound or Unstake Your Tokens

```bash
sentinelhub tx staking unbond $(sentinelhub keys show wallet --bech val -a) 1000000udvpn \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.sentinelhub\
--node tcp://localhost:16704 \
--chain-id sentinelhub-2 \
-y 
```

Send tokens to the wallet

```
sentinelhub tx bank send wallet <TO_WALLET_ADDRESS> 1000000udvpn \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.sentinelhub\
--node tcp://localhost:16704 \
--chain-id sentinelhub-2 \
-y 
```

### Governance

{% hint style="info" %}
Install jq package for management json format

**Ubuntu**

`apt install jq`

**`CentOS`**

`yum install jq`

**Arch Linux**

`pacman -S jq`
{% endhint %}

#### List All Proposal

```bash
sentinelhub query gov proposals
```

#### How to Vote

```bash
### vote yes
sentinelhub tx gov vote 1 yes \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.sentinelhub\
--node tcp://localhost:16704 \
--chain-id sentinelhub-2 \
-y 

### vote no
sentinelhub tx gov vote 1 no \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.sentinelhub\
--node tcp://localhost:16704 \
--chain-id sentinelhub-2 \
-y 

### vote abstain
sentinelhub tx gov vote 1 abstain \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.sentinelhub\
--node tcp://localhost:16704 \
--chain-id sentinelhub-2 \
-y 

### vote No With Veto
sentinelhub tx gov vote 1 nowithveto \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.sentinelhub\
--node tcp://localhost:16704 \
--chain-id sentinelhub-2 \
-y 
```
