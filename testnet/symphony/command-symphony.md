---
description: This is command about CLI Symphony
---

# Command Symphony

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



```
--gas=auto --gas-prices=800note --gas-adjustment=1.56
```
#### **Add New Key**

```bash
symphonyd keys add mywallet --home ${HOME}/.symphonyd 
```

#### **Recover Key**

With Passphrase

```bash
symphonyd keys add mywallet --recover  --home ${HOME}/.symphonyd       
```

With Keyring

```bash
symphonyd keys add mywallet --recover --keyring-backend os --home ${HOME}/.symphonyd       
```

#### **List Key**

```bash
symphonyd keys list --home ${HOME}/.symphonyd    
```

#### **Delete Key**

```bash
symphonyd keys delete mywallet --home ${HOME}/.symphonyd
```

**Export Key**

```bash
 symphonyd keys export mywallet --home ${HOME}/.symphonyd
```

#### Import Key

```
symphonyd keys import mywallet mywallet_file.backup --home ${HOME}/.symphonyd
```

#### Show All Balances Address

<pre class="language-bash"><code class="lang-bash"><strong>for mywallet in `symphonyd keys list --home ${HOME}/.symphonyd--output json| jq -r ".[] .address"`
</strong>do
   CHAIN_ID="symphony-testnet-2"
   RPC="http://localhost:16711"
   symphonyd q bank balances ${mywallet} --home ${HOME}/.symphonyd --chain-id ${CHAIN_ID} --node ${RPC}
done
</code></pre>

#### Show Balance Address

```bash
symphonyd q bank balances mywallet_public_address --home ${HOME}/.symphonyd --chain-id symphony-testnet-2 --node http://localhost:16711
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

symphonyd tx staking create-validator \
--amount=1000000note \
--pubkey=$(symphonyd tendermint show-validator --home ${HOME}/.symphonyd) \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}" \
--chain-id=symphony-testnet-2 \
--commission-rate=0.05 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1 \
--from=mywallet \
--node http://localhost:16711 \
--home ${HOME}/.symphonyd\
  --gas=auto --gas-prices=800note --gas-adjustment=1.56   \
-y
```

#### Edit Validator Identities Informations

```bash
MONIKER="NAME_OF_YOUR_VALIDATOR"
PROFILE="PGP_KEY_OF_KEYBASE"
DETAILS="Describes Your Validator"
WEBSITE="https://yourwebsite.com"

symphonyd tx staking edit-validator \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}" \
--chain-id=symphony-testnet-2 \
--commission-rate=0.05 \
--from=mywallet \
--node http://localhost:16711 \
--home ${HOME}/.symphonyd \
  --gas=auto --gas-prices=800note --gas-adjustment=1.56   \
-y
```

#### Get Validator Info

```
symphonyd status 2>&1 | jq .ValidatorInfo
```

#### Get Syncing Block

```bash
symphonyd status 2>&1 | jq .SyncInfo
```

#### Get Peer Own Node

```bash
echo $(symphonyd tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.symphonyd/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

#### Get Peer Node

```bash
curl -sS http://localhost:16701/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

#### Unjail Validator
```bash
symphonyd tx slashing unjail \
--from mywallet \
--chain-id symphony-testnet-2 \
 --home ${HOME}/.symphonyd\
 --node  http://localhost:16711 \
   --gas=auto --gas-prices=800note --gas-adjustment=1.56   \
 -y
```

#### Jail Reason

```bash
symphonyd query slashing signing-info $(symphonyd tendermint show-validator) \
 --node  http://localhost:16711 \
 --home ${HOME}/.symphonyd
```

#### List All Active Validator

```bash
symphonyd q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### List All Inactive Validator

```bash
symphonyd q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### Show Validator Details

```
symphonyd \
query staking validator \
$(symphonyd keys show \ 
                $(symphonyd keys list --home ${HOME}/.symphonyd--output json| jq -r ".[] .address" | tail -n1) \
--bech val -a) \
--chain-id symphony-testnet-2 \
--node http://localhost:16711
```

### Token Management

#### Withdraw All Reward From Validator

```bash
symphonyd tx distribution withdraw-all-rewards \
--from mywallet \
--chain-id symphony-testnet-2 \
--node http://localhost:16711 \
--home ${HOME}/.symphonyd\
  --gas=auto --gas-prices=800note --gas-adjustment=1.56   \
-y
```

#### Withdraw Commision Reward From Validator

```bash
symphonyd tx distribution withdraw-rewards $(symphonyd keys show mywallet --bech val -a) \
--commission \
--from mywallet \
--chain-id symphony-testnet-2 \
--node http://localhost:16711 \
--home ${HOME}/.symphonyd\
  --gas=auto --gas-prices=800note --gas-adjustment=1.56   \
-y 
```

#### Delegate My Token to Own Validator

```bash
symphonyd tx staking delegate $(symphonyd keys show wallet --bech val -a) 100000note \
--from mywallet \
--home ${HOME}/.symphonyd\
--node http://localhost:16711 \
--chain-id symphony-testnet-2 \
  --gas=auto --gas-prices=800note --gas-adjustment=1.56   \
-y
```

#### Delegate Your Token To Our Validator

```bash
symphonyd tx staking delegate prefixVALOPExxxxxx 100000note \ 
--from mywallet \
--home ${HOME}/.symphonyd\
--node http://localhost:16711 \
--chain-id symphony-testnet-2 \
  --gas=auto --gas-prices=800note --gas-adjustment=1.56   \
-y
```

#### Redelegate Tokens to Another Validator

```bash
symphonyd tx staking redelegate $(symphonyd keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000note \
--from mywallet 
--home ${HOME}/.symphonyd\
--node http://localhost:16711 \
--chain-id symphony-testnet-2 \
  --gas=auto --gas-prices=800note --gas-adjustment=1.56   \
-y 
```

#### Unbound or Unstake Your Tokens

```bash
symphonyd tx staking unbond $(symphonyd keys show wallet --bech val -a) 1000000note \
--from mywallet \
--home ${HOME}/.symphonyd\
--node http://localhost:16711 \
--chain-id symphony-testnet-2 \
  --gas=auto --gas-prices=800note --gas-adjustment=1.56   \
-y 
```

Send tokens to the wallet

```
symphonyd tx bank send wallet <TO_WALLET_ADDRESS> 1000000note \
--from mywallet \
--home ${HOME}/.symphonyd\
--node http://localhost:16711 \
--chain-id symphony-testnet-2 \
  --gas=auto --gas-prices=800note --gas-adjustment=1.56   \
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
symphonyd query gov proposals
```

#### How to Vote

```bash
### vote yes
symphonyd tx gov vote 1 yes \
--from mywallet \
--home ${HOME}/.symphonyd\
--node http://localhost:16711 \
--chain-id symphony-testnet-2 \
  --gas=auto --gas-prices=800note --gas-adjustment=1.56   \
-y 

### vote no
symphonyd tx gov vote 1 no \
--from mywallet \
--home ${HOME}/.symphonyd\
--node http://localhost:16711 \
--chain-id symphony-testnet-2 \
  --gas=auto --gas-prices=800note --gas-adjustment=1.56   \
-y 

### vote abstain
symphonyd tx gov vote 1 abstain \
--from mywallet \
--home ${HOME}/.symphonyd\
--node http://localhost:16711 \
--chain-id symphony-testnet-2 \
  --gas=auto --gas-prices=800note --gas-adjustment=1.56   \
-y 

### vote No With Veto
symphonyd tx gov vote 1 nowithveto \
--from mywallet \
--home ${HOME}/.symphonyd\
--node http://localhost:16711 \
--chain-id symphony-testnet-2 \
  --gas=auto --gas-prices=800note --gas-adjustment=1.56   \
-y 
```
