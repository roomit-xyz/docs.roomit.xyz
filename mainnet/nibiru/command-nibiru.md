---
description: This is command about CLI Nibiru
---

# Command Nibiru

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
--gas=1000000 --gas-prices=30000000000unibi --gas-adjustment=1.15
```
#### **Add New Key**

```bash
nibid keys add mywallet --home ${HOME}/.nibid 
```

#### **Recover Key**

With Passphrase

```bash
nibid keys add mywallet --recover  --home ${HOME}/.nibid       
```

With Keyring

```bash
nibid keys add mywallet --recover --keyring-backend os --home ${HOME}/.nibid       
```

#### **List Key**

```bash
nibid keys list --home ${HOME}/.nibid    
```

#### **Delete Key**

```bash
nibid keys delete mywallet --home ${HOME}/.nibid
```

**Export Key**

```bash
 nibid keys export mywallet --home ${HOME}/.nibid
```

#### Import Key

```
nibid keys import mywallet mywallet_file.backup --home ${HOME}/.nibid
```

#### Show All Balances Address

<pre class="language-bash"><code class="lang-bash"><strong>for mywallet in `nibid keys list --home ${HOME}/.nibid--output json| jq -r ".[] .address"`
</strong>do
   CHAIN_ID="cataclysm-1"
   RPC="http:////localhost:16706"
   nibid q bank balances ${mywallet} --home ${HOME}/.nibid --chain-id ${CHAIN_ID} --node ${RPC}
done
</code></pre>

#### Show Balance Address

```bash
nibid q bank balances mywallet_public_address --home ${HOME}/.nibid --chain-id cataclysm-1 --node http:////localhost:16706
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

nibid tx staking create-validator \
--amount=1000000unibi \
--pubkey=$(nibid tendermint show-validator --home ${HOME}/.nibid) \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}" \
--chain-id=cataclysm-1 \
--commission-rate=0.05 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1 \
--from=mywallet \
--node http:////localhost:16706 \
--home ${HOME}/.nibid\
--gas=1000000 --gas-prices=30000000000unibi --gas-adjustment=1.15 \
-y
```

#### Edit Validator Identities Informations

```bash
MONIKER="NAME_OF_YOUR_VALIDATOR"
PROFILE="PGP_KEY_OF_KEYBASE"
DETAILS="Describes Your Validator"
WEBSITE="https://yourwebsite.com"

nibid tx staking edit-validator \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}" \
--chain-id=cataclysm-1 \
--commission-rate=0.05 \
--from=mywallet \
--node http:////localhost:16706 \
--home ${HOME}/.nibid \
--gas=1000000 --gas-prices=30000000000unibi --gas-adjustment=1.15 \
-y
```

#### Get Validator Info

```
nibid status 2>&1 | jq .ValidatorInfo
```

#### Get Syncing Block

```bash
nibid status 2>&1 | jq .SyncInfo
```

#### Get Peer Own Node

```bash
echo $(nibid tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.nibid/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

#### Get Peer Node

```bash
curl -sS http://localhost:16701/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

#### Unjail Validator
```bash
nibid tx slashing unjail \
--from mywallet \
--chain-id cataclysm-1 \
 --home ${HOME}/.nibid\
 --node  http:////localhost:16706 \
 --gas=1000000 --gas-prices=30000000000unibi --gas-adjustment=1.15 \
 -y
```

#### Jail Reason

```bash
nibid query slashing signing-info $(nibid tendermint show-validator) \
 --node  http:////localhost:16706 \
 --home ${HOME}/.nibid
```

#### List All Active Validator

```bash
nibid q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### List All Inactive Validator

```bash
nibid q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### Show Validator Details

```
nibid \
query staking validator \
$(nibid keys show \ 
                $(nibid keys list --home ${HOME}/.nibid--output json| jq -r ".[] .address" | tail -n1) \
--bech val -a) \
--chain-id cataclysm-1 \
--node http:////localhost:16706
```

### Token Management

#### Withdraw All Reward From Validator

```bash
nibid tx distribution withdraw-all-rewards \
--from mywallet \
--chain-id cataclysm-1 \
--node http:////localhost:16706 \
--home ${HOME}/.nibid\
--gas=1000000 --gas-prices=30000000000unibi --gas-adjustment=1.15 \
-y
```

#### Withdraw Commision Reward From Validator

```bash
nibid tx distribution withdraw-rewards $(nibid keys show mywallet --bech val -a) \
--commission \
--from mywallet \
--chain-id cataclysm-1 \
--node http:////localhost:16706 \
--home ${HOME}/.nibid\
--gas=1000000 --gas-prices=30000000000unibi --gas-adjustment=1.15 \
-y 
```

#### Delegate My Token to Own Validator

```bash
nibid tx staking delegate $(nibid keys show wallet --bech val -a) 100000unibi \
--from mywallet \
--home ${HOME}/.nibid\
--node http:////localhost:16706 \
--chain-id cataclysm-1 \
--gas=1000000 --gas-prices=30000000000unibi --gas-adjustment=1.15 \
-y
```

#### Delegate Your Token To Our Validator

```bash
nibid tx staking delegate prefixVALOPExxxxxx 100000unibi \ 
--from mywallet \
--home ${HOME}/.nibid\
--node http:////localhost:16706 \
--chain-id cataclysm-1 \
--gas=1000000 --gas-prices=30000000000unibi --gas-adjustment=1.15 \
-y
```

#### Redelegate Tokens to Another Validator

```bash
nibid tx staking redelegate $(nibid keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000unibi \
--from mywallet 
--home ${HOME}/.nibid\
--node http:////localhost:16706 \
--chain-id cataclysm-1 \
--gas=1000000 --gas-prices=30000000000unibi --gas-adjustment=1.15 \
-y 
```

#### Unbound or Unstake Your Tokens

```bash
nibid tx staking unbond $(nibid keys show wallet --bech val -a) 1000000unibi \
--from mywallet \
--home ${HOME}/.nibid\
--node http:////localhost:16706 \
--chain-id cataclysm-1 \
--gas=1000000 --gas-prices=30000000000unibi --gas-adjustment=1.15 \
-y 
```

Send tokens to the wallet

```
nibid tx bank send wallet <TO_WALLET_ADDRESS> 1000000unibi \
--from mywallet \
--home ${HOME}/.nibid\
--node http:////localhost:16706 \
--chain-id cataclysm-1 \
--gas=1000000 --gas-prices=30000000000unibi --gas-adjustment=1.15 \
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
nibid query gov proposals
```

#### How to Vote

```bash
### vote yes
nibid tx gov vote 1 yes \
--from mywallet \
--home ${HOME}/.nibid\
--node http:////localhost:16706 \
--chain-id cataclysm-1 \
--gas=1000000 --gas-prices=30000000000unibi --gas-adjustment=1.15 \
-y 

### vote no
nibid tx gov vote 1 no \
--from mywallet \
--home ${HOME}/.nibid\
--node http:////localhost:16706 \
--chain-id cataclysm-1 \
--gas=1000000 --gas-prices=30000000000unibi --gas-adjustment=1.15 \
-y 

### vote abstain
nibid tx gov vote 1 abstain \
--from mywallet \
--home ${HOME}/.nibid\
--node http:////localhost:16706 \
--chain-id cataclysm-1 \
--gas=1000000 --gas-prices=30000000000unibi --gas-adjustment=1.15 \
-y 

### vote No With Veto
nibid tx gov vote 1 nowithveto \
--from mywallet \
--home ${HOME}/.nibid\
--node http:////localhost:16706 \
--chain-id cataclysm-1 \
--gas=1000000 --gas-prices=30000000000unibi --gas-adjustment=1.15 \
-y 
```
