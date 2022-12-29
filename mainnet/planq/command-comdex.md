---
description: This is command about CLI PlanQ
---

# Command PlanQ

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
**Running in user** : _salinem_
{% endhint %}

#### **Add New Key**

```bash
planqd keys add mywallet --home ${HOME}/.planqd      
```

#### **Recover Key**

With Passphrase

```bash
planqd keys add mywallet --recover  --home ${HOME}/.planqd            
```

With Keyring

```bash
planqd keys add mywallet --recover --keyring-backend os --home ${HOME}/.planqd            
```

#### **List Key**

```bash
planqd keys list --home ${HOME}/.planqd         
```

#### **Delete Key**

```bash
planqd keys delete mywallet --home ${HOME}/.planqd  
```

**Export Key**

```bash
 planqd keys export mywallet --home ${HOME}/.planqd  
```

#### Import Key

```
planqd keys import mywallet mywallet_file.backup --home ${HOME}/.planqd 
```

#### Show All Balances Address

<pre class="language-bash"><code class="lang-bash"><strong>for mywallet in `akash keys list --home ${HOME}/.akash --output json| jq -r ".[] .address"`
</strong>do
   CHAIN_ID="akash-1"
   RPC="tcp://localhost:16702"
   akash q bank balances ${mywallet} --home ${HOME}/.akash --chain-id ${CHAIN_ID} --node ${RPC}
done
</code></pre>

#### Show Balance Address

```bash
akash q bank balances mywallet_public_address --home ${HOME}/.akash --chain-id akash-1 --node tcp://localhost:16702
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

akash tx staking create-validator \
--amount=1000000ucmdx \
--pubkey=$(akash tendermint show-validator --home ${HOME}/.akash) \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}" \
--chain-id=akash-1 \
--commission-rate=0.05 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1 \
--from=mywallet \
--gas-adjustment=1.4 \
--gas=auto \
--node tcp://localhost:16702 \
--home ${HOME}/.akash \
-y
```

#### Edit Validator Identities Informations

```bash
MONIKER="NAME_OF_YOUR_VALIDATOR"
PROFILE="PGP_KEY_OF_KEYBASE"
DETAILS="Describes Your Validator"
WEBSITE="https://yourwebsite.com"

akash tx staking edit-validator \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}" \
--chain-id=akash-1 \
--commission-rate=0.05 \
--from=mywallet \
--gas-adjustment=1.4 \
--gas=auto \
--node tcp://localhost:16702 \
--home ${HOME}/.akash \
-y
```

#### Get Validator Info

```
akash status 2>&1 | jq .ValidatorInfo
```

#### Get Syncing Block

```
akash status 2>&1 | jq .SyncInfo
```

#### Get Peer Own Node

```bash
echo $(akash tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.akash/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

#### Get Peer Node

```
curl -sS http://localhost:16702/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

#### Unjail Validator

```bash
akash tx slashing unjail \
--from mywallet \
--chain-id akash-1 \
 --home ${HOME}/.akash \
 --node  tcp://localhost:16702 \
 --gas auto \
 --gas-adjustment 1.4 \
 -y
```

#### Jail Reason

```bash
akash query slashing signing-info $(akash tendermint show-validator) \
 --node  tcp://localhost:16702 \
 --home ${HOME}/.akash
```

#### List All Active Validator

```bash
akash q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### List All Inactive Validator

```bash
akash q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### Show Validator Details

```
akash \
query staking validator \
$(akash keys show \ 
                $(akash keys list --home ${HOME}/.akash --output json| jq -r ".[] .address" | tail -n1) \
--bech val -a) \
--chain-id akash-1 \
--node tcp://localhost:16702
```

### Token Management

#### Withdraw All Reward From Validator

```bash
akash tx distribution withdraw-all-rewards \
--from mywallet \
--chain-id akash-1 \
--node tcp://localhost:16702 \
--home ${HOME}/.akash \
--gas-adjustment 1.4 \
--gas auto \
-y
```

#### Withdraw Commision Reward From Validator

```bash
akash tx distribution withdraw-rewards $(akash keys show mywallet --bech val -a) \
--commission \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--chain-id akash-1 \
--node tcp://localhost:16702 \
--home ${HOME}/.akash \
-y 
```

#### Delegate My Token to Own Validator

```bash
akash tx staking delegate $(akash keys show wallet --bech val -a) 1000000ucmdx \
--from mywallet \
--gas-adjustment 1.4 \
--home ${HOME}/.akash \
--node tcp://localhost:16702 \
--chain-id akash-1 \
--gas auto \
-y
```

#### Delegate Your Token To Our Validator

```bash
akash tx staking delegate gravityvaloper1ssduj8c0cc8kquljvw3ygq9hduvcysnf590lmz 1000000ucmdx \ 
--from mywallet \
--gas-adjustment 1.4 \ 
--gas auto \
--home ${HOME}/.akash \
--node tcp://localhost:16702 \
--chain-id akash-1 \
-y
```

#### Redelegate Tokens to Another Validator

```bash
akash tx staking redelegate $(akash keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000ucmdx \
--from mywallet 
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.akash \
--node tcp://localhost:16702 \
--chain-id akash-1 \
-y 
```

#### Unbound or Unstake Your Tokens

```bash
akash tx staking unbond $(akash keys show wallet --bech val -a) 1000000ucmdx \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.akash \
--node tcp://localhost:16702 \
--chain-id akash-1 \
-y 
```

Send tokens to the wallet

```
akash tx bank send wallet <TO_WALLET_ADDRESS> 1000000ucmdx \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.akash \
--node tcp://localhost:16702 \
--chain-id akash-1 \
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
akash query gov proposals
```

#### How to Vote

```bash
### vote yes
akash tx gov vote 1 yes \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.akash \
--node tcp://localhost:16702 \
--chain-id akash-1 \
-y 

### vote no
akash tx gov vote 1 no \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.akash \
--node tcp://localhost:16702 \
--chain-id akash-1 \
-y 

### vote abstain
akash tx gov vote 1 abstain \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.akash \
--node tcp://localhost:16702 \
--chain-id akash-1 \
-y 

### vote No With Veto
akash tx gov vote 1 nowithveto \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.akash \
--node tcp://localhost:16702 \
--chain-id akash-1 \
-y 
```
