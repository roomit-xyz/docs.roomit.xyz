---
description: This is command about CLI Gravity Bridge
---

# Command Gravity

### **Key Management**

{% hint style="info" %}
we assume, variable name\_your\_wallet is name of your own wallet.&#x20;



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
gravityd keys add mywallet --home ${HOME}/.gravity        
```

#### **Recover Key**

With Passphrase

```bash
gravityd keys add mywallet --recover  --home ${HOME}/.gravity              
```

With Keyring

```bash
gravityd keys add mywallet --recover --keyring-backend os --home ${HOME}/.gravity              
```

#### **List Key**

```bash
gravityd keys list --home ${HOME}/.gravity           
```

#### **Delete Key**

```bash
gravityd keys delete mywallet --home ${HOME}/.gravity    
```

**Export Key**

```bash
 gravityd keys export mywallet --home ${HOME}/.gravity    
```

#### Import Key

```
gravityd keys import mywallet mywallet_file.backup --home ${HOME}/.gravity 
```

#### Show All Balances Address

<pre class="language-bash"><code class="lang-bash"><strong>for mywallet in `gravityd keys list --home ${HOME}/.gravity --output json| jq -r ".[] .address"`
</strong>do
   CHAIN_ID="gravity-bridge-3"
   RPC="tcp://localhost:16700"
   gravityd q bank balances ${mywallet} --home ${HOME}/.gravity --chain-id ${CHAIN_ID} --node ${RPC}
done
</code></pre>

#### Show Balance Address

```bash
gravityd q bank balances mywallet_public_address --home ${HOME}/.gravity --chain-id gravity-bridge-3 --node tcp://localhost:16700
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

gravityd tx staking create-validator \
--amount=1000000ugraviton \
--pubkey=$(gravityd tendermint show-validator --home ${HOME}/.gravity) \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}" \
--chain-id=gravity-bridge-3 \
--commission-rate=0.05 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1 \
--from=mywallet \
--gas-adjustment=1.4 \
--gas=auto \
--node tcp://localhost:16700 \
--home ${HOME}/.gravity \
-y
```

#### Edit Validator Identities Informations

```bash
MONIKER="NAME_OF_YOUR_VALIDATOR"
PROFILE="PGP_KEY_OF_KEYBASE"
DETAILS="Describes Your Validator"
WEBSITE="https://yourwebsite.com"

gravityd tx staking edit-validator \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}"
--chain-id=gravity-bridge-3 \
--commission-rate=0.05 \
--from=mywallet \
--gas-adjustment=1.4 \
--gas=auto \
--node tcp://localhost:16700 \
--home ${HOME}/.gravity \
-y
```

#### Get Validator Info

```
gravityd status 2>&1 | jq .ValidatorInfo
```

#### Get Syncing Block

```
gravityd status 2>&1 | jq .SyncInfo
```

#### Get Peer Node

```bash
echo $(gravityd tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.gravity/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

#### Get Peer Node

```
curl -sS http://localhost:16657/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

#### Unjail Validator

```bash
gravityd tx slashing unjail \
--from mywallet \
--chain-id gravity-bridge-3 \
 --home ${HOME}/.gravity \
 --node  tcp://localhost:16700 \
 --gas auto \
 --gas-adjustment 1.4 \
 -y
```

#### Jail Reason

```bash
gravityd query slashing signing-info $(gravityd tendermint show-validator) \
 --node  tcp://localhost:16700 \
 --home ${HOME}/.gravity
```

#### List All Active Validator

```bash
gravityd q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### List All Inactive Validator

```bash
gravityd q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### Show Validator Details

```
gravityd \
query staking validator \
$(gravityd keys show \ 
                $(gravityd keys list --home ${HOME}/.gravity --output json| jq -r ".[] .address" | tail -n1) \
--bech val -a) \
--chain-id gravity-bridge-3 \
--node tcp://localhost:16700
```

### Token Management

#### Withdraw All Reward From Validator

```bash
gravityd tx distribution withdraw-all-rewards \
--from mywallet \
--chain-id gravity-bridge-3 \
--node tcp://localhost:16700 \
--home ${HOME}/.gravity \
--gas-adjustment 1.4 \
--gas auto \
-y
```

#### Withdraw Commision Reward From Validator

```bash
gravityd tx distribution withdraw-rewards $(gravityd keys show mywallet --bech val -a) \
--commission \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--chain-id gravity-bridge-3 \
--node tcp://localhost:16700 \
--home ${HOME}/.gravity \
-y 
```

#### Delegate My Token to Own Validator

```bash
gravityd tx staking delegate $(gravityd keys show wallet --bech val -a) 1000000ugraviton \
--from mywallet \
--gas-adjustment 1.4 \
--home ${HOME}/.gravity \
--node tcp://localhost:16700 \
--chain-id gravity-bridge-3 \
--gas auto \
-y
```

#### Delegate Your Token To Our Validator

```bash
gravityd tx staking delegate gravityvaloper1ssduj8c0cc8kquljvw3ygq9hduvcysnf590lmz 1000000ugraviton \ 
--from mywallet \
--gas-adjustment 1.4 \ 
--gas auto \
--home ${HOME}/.gravity \
--node tcp://localhost:16700 \
--chain-id gravity-bridge-3 \
-y
```

#### Redelegate Tokens to Another Validator

```bash
gravityd tx staking redelegate $(gravityd keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000ugraviton \
--from mywallet 
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.gravity \
--node tcp://localhost:16700 \
--chain-id gravity-bridge-3 \
-y 
```

#### Unbound or Unstake Your Tokens

```bash
gravityd tx staking unbond $(gravityd keys show wallet --bech val -a) 1000000ugraviton \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.gravity \
--node tcp://localhost:16700 \
--chain-id gravity-bridge-3 \
-y 
```

Send tokens to the wallet

```
gravityd tx bank send wallet <TO_WALLET_ADDRESS> 1000000ugraviton \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.gravity \
--node tcp://localhost:16700 \
--chain-id gravity-bridge-3 \
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
gravityd query gov proposals
```

#### How to Vote

```bash
### vote yes
gravityd tx gov vote 1 yes \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.gravity \
--node tcp://localhost:16700 \
--chain-id gravity-bridge-3 \
-y 

### vote no
gravityd tx gov vote 1 no \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.gravity \
--node tcp://localhost:16700 \
--chain-id gravity-bridge-3 \
-y 

### vote abstain
gravityd tx gov vote 1 abstain \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.gravity \
--node tcp://localhost:16700 \
--chain-id gravity-bridge-3 \
-y 

### vote No With Veto
gravityd tx gov vote 1 nowithveto \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.gravity \
--node tcp://localhost:16700 \
--chain-id gravity-bridge-3 \
-y 
```
