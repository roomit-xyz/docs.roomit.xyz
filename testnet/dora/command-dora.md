---
description: This is command about CLI Dora
---

# Command Dora

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

#### Gas Fee
        
```
--gas=auto --gas-prices=100000000000peaka --gas-adjustment=1.6
```
#### **Add New Key**

```bash
dorad keys add mywallet --home ${HOME}/.dora 
```

#### **Recover Key**

With Passphrase

```bash
dorad keys add mywallet --recover  --home ${HOME}/.dora       
```

With Keyring

```bash
dorad keys add mywallet --recover --keyring-backend os --home ${HOME}/.dora       
```

#### **List Key**

```bash
dorad keys list --home ${HOME}/.dora    
```

#### **Delete Key**

```bash
dorad keys delete mywallet --home ${HOME}/.dora
```

**Export Key**

```bash
 dorad keys export mywallet --home ${HOME}/.dora
```

#### Import Key

```
dorad keys import mywallet mywallet_file.backup --home ${HOME}/.dora
```

#### Show All Balances Address

<pre class="language-bash"><code class="lang-bash"><strong>for mywallet in `dorad keys list --home ${HOME}/.dora--output json| jq -r ".[] .address"`
</strong>do
   CHAIN_ID="vota-sf"
   RPC="http:////localhost:26709"
   dorad q bank balances ${mywallet} --home ${HOME}/.dora --chain-id ${CHAIN_ID} --node ${RPC}
done
</code></pre>

#### Show Balance Address

```bash
dorad q bank balances mywallet_public_address --home ${HOME}/.dora --chain-id vota-sf --node http:////localhost:26709
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

dorad tx staking create-validator \
--amount=1000000000000000000peaka \
--pubkey=$(dorad tendermint show-validator --home ${HOME}/.dora) \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}" \
--chain-id=vota-sf \
--commission-rate=0.05 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1 \
--from=mywallet \
--node http:////localhost:26709 \
--home ${HOME}/.dora\
--gas=auto --gas-prices=100000000000peaka --gas-adjustment=1.6 \
-y
```

#### Edit Validator Identities Informations

```bash
MONIKER="NAME_OF_YOUR_VALIDATOR"
PROFILE="PGP_KEY_OF_KEYBASE"
DETAILS="Describes Your Validator"
WEBSITE="https://yourwebsite.com"

dorad tx staking edit-validator \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}" \
--chain-id=vota-sf \
--commission-rate=0.05 \
--from=mywallet \
--node http:////localhost:26709 \
--home ${HOME}/.dora \
--gas=auto --gas-prices=100000000000peaka --gas-adjustment=1.6 \
-y
```

#### Get Validator Info

```
dorad status 2>&1 | jq .ValidatorInfo
```

#### Get Syncing Block

```bash
dorad status 2>&1 | jq .SyncInfo
```

#### Get Peer Own Node

```bash
echo $(dorad tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.dora/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

#### Get Peer Node

```bash
curl -sS http://localhost:16701/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

#### Unjail Validator
```bash
dorad tx slashing unjail \
--from mywallet \
--chain-id vota-sf \
 --home ${HOME}/.dora\
 --node  http:////localhost:26709 \
 --gas=auto --gas-prices=100000000000peaka --gas-adjustment=1.6 \
 -y
```

#### Jail Reason

```bash
dorad query slashing signing-info $(dorad tendermint show-validator) \
 --node  http:////localhost:26709 \
 --home ${HOME}/.dora
```

#### List All Active Validator

```bash
dorad q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### List All Inactive Validator

```bash
dorad q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### Show Validator Details

```
dorad \
query staking validator \
$(dorad keys show \ 
                $(dorad keys list --home ${HOME}/.dora--output json| jq -r ".[] .address" | tail -n1) \
--bech val -a) \
--chain-id vota-sf \
--node http:////localhost:26709
```

### Token Management

#### Withdraw All Reward From Validator

```bash
dorad tx distribution withdraw-all-rewards \
--from mywallet \
--chain-id vota-sf \
--node http:////localhost:26709 \
--home ${HOME}/.dora\
--gas=auto --gas-prices=100000000000peaka --gas-adjustment=1.6 \
-y
```

#### Withdraw Commision Reward From Validator

```bash
dorad tx distribution withdraw-rewards $(dorad keys show mywallet --bech val -a) \
--commission \
--from mywallet \
--chain-id vota-sf \
--node http:////localhost:26709 \
--home ${HOME}/.dora\
--gas=auto --gas-prices=100000000000peaka --gas-adjustment=1.6 \
-y 
```

#### Delegate My Token to Own Validator

```bash
dorad tx staking delegate $(dorad keys show wallet --bech val -a) 100000peaka \
--from mywallet \
--home ${HOME}/.dora\
--node http:////localhost:26709 \
--chain-id vota-sf \
--gas=auto --gas-prices=100000000000peaka --gas-adjustment=1.6 \
-y
```

#### Delegate Your Token To Our Validator

```bash
dorad tx staking delegate prefixVALOPExxxxxx 100000peaka \ 
--from mywallet \
--home ${HOME}/.dora\
--node http:////localhost:26709 \
--chain-id vota-sf \
--gas=auto --gas-prices=100000000000peaka --gas-adjustment=1.6 \
-y
```

#### Redelegate Tokens to Another Validator

```bash
dorad tx staking redelegate $(dorad keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000peaka \
--from mywallet 
--home ${HOME}/.dora\
--node http:////localhost:26709 \
--chain-id vota-sf \
--gas=auto --gas-prices=100000000000peaka --gas-adjustment=1.6 \
-y 
```

#### Unbound or Unstake Your Tokens

```bash
dorad tx staking unbond $(dorad keys show wallet --bech val -a) 1000000peaka \
--from mywallet \
--home ${HOME}/.dora\
--node http:////localhost:26709 \
--chain-id vota-sf \
--gas=auto --gas-prices=100000000000peaka --gas-adjustment=1.6 \
-y 
```

Send tokens to the wallet

```
dorad tx bank send wallet <TO_WALLET_ADDRESS> 1000000peaka \
--from mywallet \
--home ${HOME}/.dora\
--node http:////localhost:26709 \
--chain-id vota-sf \
--gas=auto --gas-prices=100000000000peaka --gas-adjustment=1.6 \
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
dorad query gov proposals
```

#### How to Vote

```bash
### vote yes
dorad tx gov vote 1 yes \
--from mywallet \
--home ${HOME}/.dora\
--node http:////localhost:26709 \
--chain-id vota-sf \
--gas=auto --gas-prices=100000000000peaka --gas-adjustment=1.6 \
-y 

### vote no
dorad tx gov vote 1 no \
--from mywallet \
--home ${HOME}/.dora\
--node http:////localhost:26709 \
--chain-id vota-sf \
--gas=auto --gas-prices=100000000000peaka --gas-adjustment=1.6 \
-y 

### vote abstain
dorad tx gov vote 1 abstain \
--from mywallet \
--home ${HOME}/.dora\
--node http:////localhost:26709 \
--chain-id vota-sf \
--gas=auto --gas-prices=100000000000peaka --gas-adjustment=1.6 \
-y 

### vote No With Veto
dorad tx gov vote 1 nowithveto \
--from mywallet \
--home ${HOME}/.dora\
--node http:////localhost:26709 \
--chain-id vota-sf \
--gas=auto --gas-prices=100000000000peaka --gas-adjustment=1.6 \
-y 
```
