---
description: This is command about CLI Gitopia
---

# Command Gitopia

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
gitopiad keys add mywallet --home ${HOME}/.gitopia      
```

#### **Recover Key**

With Passphrase

```bash
gitopiad keys add mywallet --recover  --home ${HOME}/.gitopia            
```

With Keyring

```bash
gitopiad keys add mywallet --recover --keyring-backend os --home ${HOME}/.gitopia            
```

#### **List Key**

```bash
gitopiad keys list --home ${HOME}/.gitopia         
```

#### **Delete Key**

```bash
gitopiad keys delete mywallet --home ${HOME}/.gitopia  
```

**Export Key**

```bash
 gitopiad keys export mywallet --home ${HOME}/.gitopia  
```

#### Import Key

```
gitopiad keys import mywallet mywallet_file.backup --home ${HOME}/.gitopia 
```

#### Show All Balances Address

<pre class="language-bash"><code class="lang-bash"><strong>for mywallet in `gitopiad keys list --home ${HOME}/.gitopia --output json| jq -r ".[] .address"`
</strong>do
   CHAIN_ID="gitopia"
   RPC="tcp://localhost:16701"
   gitopiad q bank balances ${mywallet} --home ${HOME}/.gitopia --chain-id ${CHAIN_ID} --node ${RPC}
done
</code></pre>

#### Show Balance Address

```bash
gitopiad q bank balances mywallet_public_address --home ${HOME}/.gitopia --chain-id gitopia --node tcp://localhost:16701
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

gitopiad tx staking create-validator \
--amount=1000000ulore \
--pubkey=$(gitopiad tendermint show-validator --home ${HOME}/.gitopiad) \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}" \
--chain-id=gitopia \
--commission-rate=0.05 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1 \
--from=mywallet \
--gas-adjustment=1.4 \
--gas=auto \
--node tcp://localhost:16701 \
--home ${HOME}/.gitopia \
-y
```

#### Edit Validator Identities Informations

```bash
MONIKER="NAME_OF_YOUR_VALIDATOR"
PROFILE="PGP_KEY_OF_KEYBASE"
DETAILS="Describes Your Validator"
WEBSITE="https://yourwebsite.com"

gitopiad tx staking edit-validator \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}" \
--chain-id=gitopia \
--commission-rate=0.05 \
--from=mywallet \
--gas-adjustment=1.4 \
--gas=auto \
--node tcp://localhost:16701 \
--home ${HOME}/.gitopia \
-y
```

#### Get Validator Info

```
gitopiad status 2>&1 | jq .ValidatorInfo
```

#### Get Syncing Block

```
gitopiad status 2>&1 | jq .SyncInfo
```

#### Get Peer Own Node

```bash
echo $(gitopiad tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.gitopiad/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

#### Get Peer Node

```
curl -sS http://localhost:16701/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

#### Unjail Validator

```bash
gitopiad tx slashing unjail \
--from mywallet \
--chain-id gitopia \
 --home ${HOME}/.gitopia \
 --node  tcp://localhost:16701 \
 --gas auto \
 --gas-adjustment 1.4 \
 -y
```

#### Jail Reason

```bash
gitopiad query slashing signing-info $(gitopiad tendermint show-validator) \
 --node  tcp://localhost:16701 \
 --home ${HOME}/.gitopiad
```

#### List All Active Validator

```bash
gitopiad q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### List All Inactive Validator

```bash
gitopiad q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### Show Validator Details

```
gitopiad \
query staking validator \
$(gitopiad keys show \ 
                $(gitopiad keys list --home ${HOME}/.gitopia --output json| jq -r ".[] .address" | tail -n1) \
--bech val -a) \
--chain-id gitopia \
--node tcp://localhost:16701
```

### Token Management

#### Withdraw All Reward From Validator

```bash
gitopiad tx distribution withdraw-all-rewards \
--from mywallet \
--chain-id gitopia \
--node tcp://localhost:16701 \
--home ${HOME}/.gitopia \
--gas-adjustment 1.4 \
--gas auto \
-y
```

#### Withdraw Commision Reward From Validator

```bash
gitopiad tx distribution withdraw-rewards $(gitopiad keys show mywallet --bech val -a) \
--commission \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--chain-id gitopia \
--node tcp://localhost:16701 \
--home ${HOME}/.gitopia \
-y 
```

#### Delegate My Token to Own Validator

```bash
gitopiad tx staking delegate $(gitopiad keys show wallet --bech val -a) 1000000ulore \
--from mywallet \
--gas-adjustment 1.4 \
--home ${HOME}/.gitopia \
--node tcp://localhost:16701 \
--chain-id gitopia \
--gas auto \
-y
```

#### Delegate Your Token To Our Validator

```bash
gitopiad tx staking delegate gitopiavaloper1pv8fkl4t7wk9mwptkwf8pemy9rt8qpkydr6k3p 1000000ulore \ 
--from mywallet \
--gas-adjustment 1.4 \ 
--gas auto \
--home ${HOME}/.gitopia \
--node tcp://localhost:16701 \
--chain-id gitopia \
-y
```

#### Redelegate Tokens to Another Validator

```bash
gitopiad tx staking redelegate $(gitopiad keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000ulore \
--from mywallet 
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.gitopia \
--node tcp://localhost:16701 \
--chain-id gitopia \
-y 
```

#### Unbound or Unstake Your Tokens

```bash
gitopiad tx staking unbond $(gitopiad keys show wallet --bech val -a) 1000000ulore \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.gitopia \
--node tcp://localhost:16701 \
--chain-id gitopia \
-y 
```

Send tokens to the wallet

```
gitopiad tx bank send wallet <TO_WALLET_ADDRESS> 1000000ulore \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.gitopia \
--node tcp://localhost:16701 \
--chain-id gitopia \
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
gitopiad query gov proposals
```

#### How to Vote

```bash
### vote yes
gitopiad tx gov vote 1 yes \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.gitopia \
--node tcp://localhost:16701 \
--chain-id gitopia \
-y 

### vote no
gitopiad tx gov vote 1 no \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.gitopia \
--node tcp://localhost:16701 \
--chain-id gitopia \
-y 

### vote abstain
gitopiad tx gov vote 1 abstain \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.gitopia \
--node tcp://localhost:16701 \
--chain-id gitopia \
-y 

### vote No With Veto
gitopiad tx gov vote 1 nowithveto \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.gitopia \
--node tcp://localhost:16701 \
--chain-id gitopia \
-y 
```
