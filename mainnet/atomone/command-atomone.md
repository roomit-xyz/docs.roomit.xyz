---
description: This is command about CLI Atomone
---

# Command Atomone

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
--gas=auto --gas-prices=0.025uatone --gas-adjustment=1.5
```
#### **Add New Key**

```bash
atomoned keys add mywallet --home ${HOME}/.atomone 
```

#### **Recover Key**

With Passphrase

```bash
atomoned keys add mywallet --recover  --home ${HOME}/.atomone       
```

With Keyring

```bash
atomoned keys add mywallet --recover --keyring-backend os --home ${HOME}/.atomone       
```

#### **List Key**

```bash
atomoned keys list --home ${HOME}/.atomone    
```

#### **Delete Key**

```bash
atomoned keys delete mywallet --home ${HOME}/.atomone
```

**Export Key**

```bash
 atomoned keys export mywallet --home ${HOME}/.atomone
```

#### Import Key

```
atomoned keys import mywallet mywallet_file.backup --home ${HOME}/.atomone
```

#### Show All Balances Address

<pre class="language-bash"><code class="lang-bash"><strong>for mywallet in `atomoned keys list --home ${HOME}/.atomone--output json| jq -r ".[] .address"`
</strong>do
   CHAIN_ID="atomone-1"
   RPC="http://localhost:16711"
   atomoned q bank balances ${mywallet} --home ${HOME}/.atomone --chain-id ${CHAIN_ID} --node ${RPC}
done
</code></pre>

#### Show Balance Address

```bash
atomoned q bank balances mywallet_public_address --home ${HOME}/.atomone --chain-id atomone-1 --node http://localhost:16711
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

atomoned tx staking create-validator \
--amount=1000000uatone \
--pubkey=$(atomoned tendermint show-validator --home ${HOME}/.atomone) \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}" \
--chain-id=atomone-1 \
--commission-rate=0.05 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1 \
--from=mywallet \
--node http://localhost:16711 \
--home ${HOME}/.atomone\
  --gas=auto --gas-prices=0.025uatone --gas-adjustment=1.5   \
-y
```

#### Edit Validator Identities Informations

```bash
MONIKER="NAME_OF_YOUR_VALIDATOR"
PROFILE="PGP_KEY_OF_KEYBASE"
DETAILS="Describes Your Validator"
WEBSITE="https://yourwebsite.com"

atomoned tx staking edit-validator \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}" \
--chain-id=atomone-1 \
--commission-rate=0.05 \
--from=mywallet \
--node http://localhost:16711 \
--home ${HOME}/.atomone \
  --gas=auto --gas-prices=0.025uatone --gas-adjustment=1.5   \
-y
```

#### Get Validator Info

```
atomoned status 2>&1 | jq .ValidatorInfo
```

#### Get Syncing Block

```bash
atomoned status 2>&1 | jq .SyncInfo
```

#### Get Peer Own Node

```bash
echo $(atomoned tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.atomone/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

#### Get Peer Node

```bash
curl -sS http://localhost:16701/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

#### Unjail Validator
```bash
atomoned tx slashing unjail \
--from mywallet \
--chain-id atomone-1 \
 --home ${HOME}/.atomone\
 --node  http://localhost:16711 \
   --gas=auto --gas-prices=0.025uatone --gas-adjustment=1.5   \
 -y
```

#### Jail Reason

```bash
atomoned query slashing signing-info $(atomoned tendermint show-validator) \
 --node  http://localhost:16711 \
 --home ${HOME}/.atomone
```

#### List All Active Validator

```bash
atomoned q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### List All Inactive Validator

```bash
atomoned q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### Show Validator Details

```
atomoned \
query staking validator \
$(atomoned keys show \ 
                $(atomoned keys list --home ${HOME}/.atomone--output json| jq -r ".[] .address" | tail -n1) \
--bech val -a) \
--chain-id atomone-1 \
--node http://localhost:16711
```

### Token Management

#### Withdraw All Reward From Validator

```bash
atomoned tx distribution withdraw-all-rewards \
--from mywallet \
--chain-id atomone-1 \
--node http://localhost:16711 \
--home ${HOME}/.atomone\
  --gas=auto --gas-prices=0.025uatone --gas-adjustment=1.5   \
-y
```

#### Withdraw Commision Reward From Validator

```bash
atomoned tx distribution withdraw-rewards $(atomoned keys show mywallet --bech val -a) \
--commission \
--from mywallet \
--chain-id atomone-1 \
--node http://localhost:16711 \
--home ${HOME}/.atomone\
  --gas=auto --gas-prices=0.025uatone --gas-adjustment=1.5   \
-y 
```

#### Delegate My Token to Own Validator

```bash
atomoned tx staking delegate $(atomoned keys show wallet --bech val -a) 100000uatone \
--from mywallet \
--home ${HOME}/.atomone\
--node http://localhost:16711 \
--chain-id atomone-1 \
  --gas=auto --gas-prices=0.025uatone --gas-adjustment=1.5   \
-y
```

#### Delegate Your Token To Our Validator

```bash
atomoned tx staking delegate prefixVALOPExxxxxx 100000uatone \ 
--from mywallet \
--home ${HOME}/.atomone\
--node http://localhost:16711 \
--chain-id atomone-1 \
  --gas=auto --gas-prices=0.025uatone --gas-adjustment=1.5   \
-y
```

#### Redelegate Tokens to Another Validator

```bash
atomoned tx staking redelegate $(atomoned keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000uatone \
--from mywallet 
--home ${HOME}/.atomone\
--node http://localhost:16711 \
--chain-id atomone-1 \
  --gas=auto --gas-prices=0.025uatone --gas-adjustment=1.5   \
-y 
```

#### Unbound or Unstake Your Tokens

```bash
atomoned tx staking unbond $(atomoned keys show wallet --bech val -a) 1000000uatone \
--from mywallet \
--home ${HOME}/.atomone\
--node http://localhost:16711 \
--chain-id atomone-1 \
  --gas=auto --gas-prices=0.025uatone --gas-adjustment=1.5   \
-y 
```

Send tokens to the wallet

```
atomoned tx bank send wallet <TO_WALLET_ADDRESS> 1000000uatone \
--from mywallet \
--home ${HOME}/.atomone\
--node http://localhost:16711 \
--chain-id atomone-1 \
  --gas=auto --gas-prices=0.025uatone --gas-adjustment=1.5   \
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
atomoned query gov proposals
```

#### How to Vote

```bash
### vote yes
atomoned tx gov vote 1 yes \
--from mywallet \
--home ${HOME}/.atomone\
--node http://localhost:16711 \
--chain-id atomone-1 \
  --gas=auto --gas-prices=0.025uatone --gas-adjustment=1.5   \
-y 

### vote no
atomoned tx gov vote 1 no \
--from mywallet \
--home ${HOME}/.atomone\
--node http://localhost:16711 \
--chain-id atomone-1 \
  --gas=auto --gas-prices=0.025uatone --gas-adjustment=1.5   \
-y 

### vote abstain
atomoned tx gov vote 1 abstain \
--from mywallet \
--home ${HOME}/.atomone\
--node http://localhost:16711 \
--chain-id atomone-1 \
  --gas=auto --gas-prices=0.025uatone --gas-adjustment=1.5   \
-y 

### vote No With Veto
atomoned tx gov vote 1 nowithveto \
--from mywallet \
--home ${HOME}/.atomone\
--node http://localhost:16711 \
--chain-id atomone-1 \
  --gas=auto --gas-prices=0.025uatone --gas-adjustment=1.5   \
-y 
```
