---
description: This is command about CLI Source Protocol
---

# Command Source

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
sourced keys add mywallet --home ${HOME}/.source    
```

#### **Recover Key**

With Passphrase

```bash
sourced keys add mywallet --recover  --home ${HOME}/.source          
```

With Keyring

```bash
sourced keys add mywallet --recover --keyring-backend os --home ${HOME}/.source          
```

#### **List Key**

```bash
sourced keys list --home ${HOME}/.source       
```

#### **Delete Key**

```bash
sourced keys delete mywallet --home ${HOME}/.source
```

**Export Key**

```bash
 sourced keys export mywallet --home ${HOME}/.source
```

#### Import Key

```
sourced keys import mywallet mywallet_file.backup --home ${HOME}/.source
```

#### Show All Balances Address

<pre class="language-bash"><code class="lang-bash"><strong>for mywallet in `sourced keys list --home ${HOME}/.source--output json| jq -r ".[] .address"`
</strong>do
   CHAIN_ID="source-1"
   RPC="tcp://localhost:16702"
   sourced q bank balances ${mywallet} --home ${HOME}/.source--chain-id ${CHAIN_ID} --node ${RPC}
done
</code></pre>

#### Show Balance Address

```bash
sourced q bank balances mywallet_public_address --home ${HOME}/.source--chain-id source-1 --node tcp://localhost:16702
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

sourced tx staking create-validator \
--amount=1000000usource \
--pubkey=$(sourced tendermint show-validator --home ${HOME}/.source) \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}" \
--chain-id=source-1 \
--commission-rate=0.05 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1 \
--from=mywallet \
--gas-adjustment=1.4 \
--gas=auto \
--node tcp://localhost:16702 \
--home ${HOME}/.source\
-y
```

#### Edit Validator Identities Informations

```bash
MONIKER="NAME_OF_YOUR_VALIDATOR"
PROFILE="PGP_KEY_OF_KEYBASE"
DETAILS="Describes Your Validator"
WEBSITE="https://yourwebsite.com"

sourced tx staking edit-validator \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}" \
--chain-id=source-1 \
--commission-rate=0.05 \
--from=mywallet \
--gas-adjustment=1.4 \
--gas=auto \
--node tcp://localhost:16702 \
--home ${HOME}/.source\
-y
```

#### Get Validator Info

```
sourced status 2>&1 | jq .ValidatorInfo
```

#### Get Syncing Block

```
sourced status 2>&1 | jq .SyncInfo
```

#### Get Peer Own Node

```bash
echo $(sourced tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.source/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

#### Get Peer Node

```
curl -sS http://localhost:16701/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

#### Unjail Validator

```bash
sourced tx slashing unjail \
--from mywallet \
--chain-id source-1 \
 --home ${HOME}/.source\
 --node  tcp://localhost:16702 \
 --gas auto \
 --gas-adjustment 1.4 \
 -y
```

#### Jail Reason

```bash
sourced query slashing signing-info $(sourced tendermint show-validator) \
 --node  tcp://localhost:16702 \
 --home ${HOME}/.source
```

#### List All Active Validator

```bash
sourced q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### List All Inactive Validator

```bash
sourced q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### Show Validator Details

```
sourced \
query staking validator \
$(sourced keys show \ 
                $(sourced keys list --home ${HOME}/.source--output json| jq -r ".[] .address" | tail -n1) \
--bech val -a) \
--chain-id source-1 \
--node tcp://localhost:16702
```

### Token Management

#### Withdraw All Reward From Validator

```bash
sourced tx distribution withdraw-all-rewards \
--from mywallet \
--chain-id source-1 \
--node tcp://localhost:16702 \
--home ${HOME}/.source\
--gas-adjustment 1.4 \
--gas auto \
-y
```

#### Withdraw Commision Reward From Validator

```bash
sourced tx distribution withdraw-rewards $(sourced keys show mywallet --bech val -a) \
--commission \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--chain-id source-1 \
--node tcp://localhost:16702 \
--home ${HOME}/.source\
-y 
```

#### Delegate My Token to Own Validator

```bash
sourced tx staking delegate $(sourced keys show wallet --bech val -a) 1000000usource \
--from mywallet \
--gas-adjustment 1.4 \
--home ${HOME}/.source\
--node tcp://localhost:16702 \
--chain-id source-1 \
--gas auto \
-y
```

#### Delegate Your Token To Our Validator

```bash
sourced tx staking delegate sourcevaloper1s2rjwh8jahg7vjac9hnj99rlkgrpeknwd8expt 1000000usource \ 
--from mywallet \
--gas-adjustment 1.4 \ 
--gas auto \
--home ${HOME}/.source\
--node tcp://localhost:16702 \
--chain-id source-1 \
-y
```

#### Redelegate Tokens to Another Validator

```bash
sourced tx staking redelegate $(sourced keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000usource \
--from mywallet 
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.source\
--node tcp://localhost:16702 \
--chain-id source-1 \
-y 
```

#### Unbound or Unstake Your Tokens

```bash
sourced tx staking unbond $(sourced keys show wallet --bech val -a) 1000000usource \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.source\
--node tcp://localhost:16702 \
--chain-id source-1 \
-y 
```

Send tokens to the wallet

```
sourced tx bank send wallet <TO_WALLET_ADDRESS> 1000000usource \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.source\
--node tcp://localhost:16702 \
--chain-id source-1 \
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
sourced query gov proposals
```

#### How to Vote

```bash
### vote yes
sourced tx gov vote 1 yes \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.source\
--node tcp://localhost:16702 \
--chain-id source-1 \
-y 

### vote no
sourced tx gov vote 1 no \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.source\
--node tcp://localhost:16702 \
--chain-id source-1 \
-y 

### vote abstain
sourced tx gov vote 1 abstain \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.source\
--node tcp://localhost:16702 \
--chain-id source-1 \
-y 

### vote No With Veto
sourced tx gov vote 1 nowithveto \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.source\
--node tcp://localhost:16702 \
--chain-id source-1 \
-y 
```
