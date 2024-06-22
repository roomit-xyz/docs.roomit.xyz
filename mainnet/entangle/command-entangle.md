---
description: This is command about CLI Entangle
---

# Command Entangle

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
--gas=500000 --gas-prices=10aNGL --gas-adjustment=1.2
```
#### **Add New Key**

```bash
entangled keys add mywallet --home ${HOME}/.entangled 
```

#### **Recover Key**

With Passphrase

```bash
entangled keys add mywallet --recover  --home ${HOME}/.entangled       
```

With Keyring

```bash
entangled keys add mywallet --recover --keyring-backend os --home ${HOME}/.entangled       
```

#### **List Key**

```bash
entangled keys list --home ${HOME}/.entangled    
```

#### **Delete Key**

```bash
entangled keys delete mywallet --home ${HOME}/.entangled
```

**Export Key**

```bash
 entangled keys export mywallet --home ${HOME}/.entangled
```

#### Import Key

```
entangled keys import mywallet mywallet_file.backup --home ${HOME}/.entangled
```

#### Show All Balances Address

<pre class="language-bash"><code class="lang-bash"><strong>for mywallet in `entangled keys list --home ${HOME}/.entangled--output json| jq -r ".[] .address"`
</strong>do
   CHAIN_ID="entangle_33033-1"
   RPC="http://localhost:16708"
   entangled q bank balances ${mywallet} --home ${HOME}/.entangled --chain-id ${CHAIN_ID} --node ${RPC}
done
</code></pre>

#### Show Balance Address

```bash
entangled q bank balances mywallet_public_address --home ${HOME}/.entangled --chain-id entangle_33033-1 --node http://localhost:16708
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

entangled tx staking create-validator \
--amount=1000000000000000000aNGL \
--pubkey=$(entangled tendermint show-validator --home ${HOME}/.entangled) \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}" \
--chain-id=entangle_33033-1 \
--commission-rate=0.05 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1 \
--from=mywallet \
--node http://localhost:16708 \
--home ${HOME}/.entangled\
--gas=500000 --gas-prices=10aNGL --gas-adjustment=1.2 \
-y
```

#### Edit Validator Identities Informations

```bash
MONIKER="NAME_OF_YOUR_VALIDATOR"
PROFILE="PGP_KEY_OF_KEYBASE"
DETAILS="Describes Your Validator"
WEBSITE="https://yourwebsite.com"

entangled tx staking edit-validator \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}" \
--chain-id=entangle_33033-1 \
--commission-rate=0.05 \
--from=mywallet \
--node http://localhost:16708 \
--home ${HOME}/.entangled \
--gas=500000 --gas-prices=10aNGL --gas-adjustment=1.2 \
-y
```

#### Get Validator Info

```
entangled status 2>&1 | jq .ValidatorInfo
```

#### Get Syncing Block

```bash
entangled status 2>&1 | jq .SyncInfo
```

#### Get Peer Own Node

```bash
echo $(entangled tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.entangled/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

#### Get Peer Node

```bash
curl -sS http://localhost:16701/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

#### Unjail Validator
```bash
entangled tx slashing unjail \
--from mywallet \
--chain-id entangle_33033-1 \
 --home ${HOME}/.entangled\
 --node  http://localhost:16708 \
 --gas=500000 --gas-prices=10aNGL --gas-adjustment=1.2 \
 -y
```

#### Jail Reason

```bash
entangled query slashing signing-info $(entangled tendermint show-validator) \
 --node  http://localhost:16708 \
 --home ${HOME}/.entangled
```

#### List All Active Validator

```bash
entangled q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### List All Inactive Validator

```bash
entangled q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### Show Validator Details

```
entangled \
query staking validator \
$(entangled keys show \ 
                $(entangled keys list --home ${HOME}/.entangled--output json| jq -r ".[] .address" | tail -n1) \
--bech val -a) \
--chain-id entangle_33033-1 \
--node http://localhost:16708
```

### Token Management

#### Withdraw All Reward From Validator

```bash
entangled tx distribution withdraw-all-rewards \
--from mywallet \
--chain-id entangle_33033-1 \
--node http://localhost:16708 \
--home ${HOME}/.entangled\
--gas=500000 --gas-prices=10aNGL --gas-adjustment=1.2 \
-y
```

#### Withdraw Commision Reward From Validator

```bash
entangled tx distribution withdraw-rewards $(entangled keys show mywallet --bech val -a) \
--commission \
--from mywallet \
--chain-id entangle_33033-1 \
--node http://localhost:16708 \
--home ${HOME}/.entangled\
--gas=500000 --gas-prices=10aNGL --gas-adjustment=1.2 \
-y 
```

#### Delegate My Token to Own Validator

```bash
entangled tx staking delegate $(entangled keys show wallet --bech val -a) 100000aNGL \
--from mywallet \
--home ${HOME}/.entangled\
--node http://localhost:16708 \
--chain-id entangle_33033-1 \
--gas=500000 --gas-prices=10aNGL --gas-adjustment=1.2 \
-y
```

#### Delegate Your Token To Our Validator

```bash
entangled tx staking delegate prefixVALOPExxxxxx 100000aNGL \ 
--from mywallet \
--home ${HOME}/.entangled\
--node http://localhost:16708 \
--chain-id entangle_33033-1 \
--gas=500000 --gas-prices=10aNGL --gas-adjustment=1.2 \
-y
```

#### Redelegate Tokens to Another Validator

```bash
entangled tx staking redelegate $(entangled keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000aNGL \
--from mywallet 
--home ${HOME}/.entangled\
--node http://localhost:16708 \
--chain-id entangle_33033-1 \
--gas=500000 --gas-prices=10aNGL --gas-adjustment=1.2 \
-y 
```

#### Unbound or Unstake Your Tokens

```bash
entangled tx staking unbond $(entangled keys show wallet --bech val -a) 1000000aNGL \
--from mywallet \
--home ${HOME}/.entangled\
--node http://localhost:16708 \
--chain-id entangle_33033-1 \
--gas=500000 --gas-prices=10aNGL --gas-adjustment=1.2 \
-y 
```

Send tokens to the wallet

```
entangled tx bank send wallet <TO_WALLET_ADDRESS> 1000000aNGL \
--from mywallet \
--home ${HOME}/.entangled\
--node http://localhost:16708 \
--chain-id entangle_33033-1 \
--gas=500000 --gas-prices=10aNGL --gas-adjustment=1.2 \
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
entangled query gov proposals
```

#### How to Vote

```bash
### vote yes
entangled tx gov vote 1 yes \
--from mywallet \
--home ${HOME}/.entangled\
--node http://localhost:16708 \
--chain-id entangle_33033-1 \
--gas=500000 --gas-prices=10aNGL --gas-adjustment=1.2 \
-y 

### vote no
entangled tx gov vote 1 no \
--from mywallet \
--home ${HOME}/.entangled\
--node http://localhost:16708 \
--chain-id entangle_33033-1 \
--gas=500000 --gas-prices=10aNGL --gas-adjustment=1.2 \
-y 

### vote abstain
entangled tx gov vote 1 abstain \
--from mywallet \
--home ${HOME}/.entangled\
--node http://localhost:16708 \
--chain-id entangle_33033-1 \
--gas=500000 --gas-prices=10aNGL --gas-adjustment=1.2 \
-y 

### vote No With Veto
entangled tx gov vote 1 nowithveto \
--from mywallet \
--home ${HOME}/.entangled\
--node http://localhost:16708 \
--chain-id entangle_33033-1 \
--gas=500000 --gas-prices=10aNGL --gas-adjustment=1.2 \
-y 
```
