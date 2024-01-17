---
description: This is command about CLI SGE Protocol
---

# Command SGE

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
sged keys add mywallet --home ${HOME}/.sge  
```

#### **Recover Key**

With Passphrase

```bash
sged keys add mywallet --recover  --home ${HOME}/.sge        
```

With Keyring

```bash
sged keys add mywallet --recover --keyring-backend os --home ${HOME}/.sge        
```

#### **List Key**

```bash
sged keys list --home ${HOME}/.sge     
```

#### **Delete Key**

```bash
sged keys delete mywallet --home ${HOME}/.sge
```

**Export Key**

```bash
 sged keys export mywallet --home ${HOME}/.sge
```

#### Import Key

```
sged keys import mywallet mywallet_file.backup --home ${HOME}/.sge
```

#### Show All Balances Address

<pre class="language-bash"><code class="lang-bash"><strong>for mywallet in `sged keys list --home ${HOME}/.sge--output json| jq -r ".[] .address"`
</strong>do
   CHAIN_ID="sgenet-1"
   RPC="tcp://localhost:16705"
   sged q bank balances ${mywallet} --home ${HOME}/.sge--chain-id ${CHAIN_ID} --node ${RPC}
done
</code></pre>

#### Show Balance Address

```bash
sged q bank balances mywallet_public_address --home ${HOME}/.sge--chain-id sgenet-1 --node tcp://localhost:16705
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

sged tx staking create-validator \
--amount=1000000usge \
--pubkey=$(sged tendermint show-validator --home ${HOME}/.sge) \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}" \
--chain-id=sgenet-1 \
--commission-rate=0.05 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1 \
--from=mywallet \
--gas-adjustment=1.4 \
--gas=auto \
--node tcp://localhost:16705 \
--home ${HOME}/.sge\
-y
```

#### Edit Validator Identities Informations

```bash
MONIKER="NAME_OF_YOUR_VALIDATOR"
PROFILE="PGP_KEY_OF_KEYBASE"
DETAILS="Describes Your Validator"
WEBSITE="https://yourwebsite.com"

sged tx staking edit-validator \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}" \
--chain-id=sgenet-1 \
--commission-rate=0.05 \
--from=mywallet \
--gas-adjustment=1.4 \
--gas=auto \
--node tcp://localhost:16705 \
--home ${HOME}/.sge\
-y
```

#### Get Validator Info

```
sged status 2>&1 | jq .ValidatorInfo
```

#### Get Syncing Block

```
sged status 2>&1 | jq .SyncInfo
```

#### Get Peer Own Node

```bash
echo $(sged tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.sge/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

#### Get Peer Node

```
curl -sS http://localhost:16701/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

#### Unjail Validator

```bash
sged tx slashing unjail \
--from mywallet \
--chain-id sgenet-1 \
 --home ${HOME}/.sge\
 --node  tcp://localhost:16705 \
 --gas auto \
 --gas-adjustment 1.4 \
 -y
```

#### Jail Reason

```bash
sged query slashing signing-info $(sged tendermint show-validator) \
 --node  tcp://localhost:16705 \
 --home ${HOME}/.sge
```

#### List All Active Validator

```bash
sged q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### List All Inactive Validator

```bash
sged q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### Show Validator Details

```
sged \
query staking validator \
$(sged keys show \ 
                $(sged keys list --home ${HOME}/.sge--output json| jq -r ".[] .address" | tail -n1) \
--bech val -a) \
--chain-id sgenet-1 \
--node tcp://localhost:16705
```

### Token Management

#### Withdraw All Reward From Validator

```bash
sged tx distribution withdraw-all-rewards \
--from mywallet \
--chain-id sgenet-1 \
--node tcp://localhost:16705 \
--home ${HOME}/.sge\
--gas-adjustment 1.4 \
--gas auto \
-y
```

#### Withdraw Commision Reward From Validator

```bash
sged tx distribution withdraw-rewards $(sged keys show mywallet --bech val -a) \
--commission \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--chain-id sgenet-1 \
--node tcp://localhost:16705 \
--home ${HOME}/.sge\
-y 
```

#### Delegate My Token to Own Validator

```bash
sged tx staking delegate $(sged keys show wallet --bech val -a) 1000000usge \
--from mywallet \
--gas-adjustment 1.4 \
--home ${HOME}/.sge\
--node tcp://localhost:16705 \
--chain-id sgenet-1 \
--gas auto \
-y
```

#### Delegate Your Token To Our Validator

```bash
sged tx staking delegate sgevaloper1rhmrwq4xket2ua4s7nqunnne8kzfz4w34082zv 1000000usge \ 
--from mywallet \
--gas-adjustment 1.4 \ 
--gas auto \
--home ${HOME}/.sge\
--node tcp://localhost:16705 \
--chain-id sgenet-1 \
-y
```

#### Redelegate Tokens to Another Validator

```bash
sged tx staking redelegate $(sged keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000usge \
--from mywallet 
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.sge\
--node tcp://localhost:16705 \
--chain-id sgenet-1 \
-y 
```

#### Unbound or Unstake Your Tokens

```bash
sged tx staking unbond $(sged keys show wallet --bech val -a) 1000000usge \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.sge\
--node tcp://localhost:16705 \
--chain-id sgenet-1 \
-y 
```

Send tokens to the wallet

```
sged tx bank send wallet <TO_WALLET_ADDRESS> 1000000usge \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.sge\
--node tcp://localhost:16705 \
--chain-id sgenet-1 \
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
sged query gov proposals
```

#### How to Vote

```bash
### vote yes
sged tx gov vote 1 yes \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.sge\
--node tcp://localhost:16705 \
--chain-id sgenet-1 \
-y 

### vote no
sged tx gov vote 1 no \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.sge\
--node tcp://localhost:16705 \
--chain-id sgenet-1 \
-y 

### vote abstain
sged tx gov vote 1 abstain \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.sge\
--node tcp://localhost:16705 \
--chain-id sgenet-1 \
-y 

### vote No With Veto
sged tx gov vote 1 nowithveto \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.sge\
--node tcp://localhost:16705 \
--chain-id sgenet-1 \
-y 
```
