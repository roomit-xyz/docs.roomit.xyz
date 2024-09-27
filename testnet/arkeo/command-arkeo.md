---
description: This is command about CLI Arkeo
---

# Command Arkeo

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
--gas=auto --gas-prices=0.1uarkeo --gas-adjustment=1.4
```
#### **Add New Key**

```bash
arkeod keys add mywallet --home ${HOME}/.arkeo 
```

#### **Recover Key**

With Passphrase

```bash
arkeod keys add mywallet --recover  --home ${HOME}/.arkeo       
```

With Keyring

```bash
arkeod keys add mywallet --recover --keyring-backend os --home ${HOME}/.arkeo       
```

#### **List Key**

```bash
arkeod keys list --home ${HOME}/.arkeo    
```

#### **Delete Key**

```bash
arkeod keys delete mywallet --home ${HOME}/.arkeo
```

**Export Key**

```bash
 arkeod keys export mywallet --home ${HOME}/.arkeo
```

#### Import Key

```
arkeod keys import mywallet mywallet_file.backup --home ${HOME}/.arkeo
```

#### Show All Balances Address

<pre class="language-bash"><code class="lang-bash"><strong>for mywallet in `arkeod keys list --home ${HOME}/.arkeo--output json| jq -r ".[] .address"`
</strong>do
   CHAIN_ID="arkeo"
   RPC="http://localhost:26700"
   arkeod q bank balances ${mywallet} --home ${HOME}/.arkeo --chain-id ${CHAIN_ID} --node ${RPC}
done
</code></pre>

#### Show Balance Address

```bash
arkeod q bank balances mywallet_public_address --home ${HOME}/.arkeo --chain-id arkeo --node http://localhost:26700
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

arkeod tx staking create-validator \
--amount=1000000uarkeo \
--pubkey=$(arkeod tendermint show-validator --home ${HOME}/.arkeo) \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}" \
--chain-id=arkeo \
--commission-rate=0.05 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1 \
--from=mywallet \
--node http://localhost:26700 \
--home ${HOME}/.arkeo\
  --gas=auto --gas-prices=0.1uarkeo --gas-adjustment=1.4   \
-y
```

#### Edit Validator Identities Informations

```bash
MONIKER="NAME_OF_YOUR_VALIDATOR"
PROFILE="PGP_KEY_OF_KEYBASE"
DETAILS="Describes Your Validator"
WEBSITE="https://yourwebsite.com"

arkeod tx staking edit-validator \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}" \
--chain-id=arkeo \
--commission-rate=0.05 \
--from=mywallet \
--node http://localhost:26700 \
--home ${HOME}/.arkeo \
  --gas=auto --gas-prices=0.1uarkeo --gas-adjustment=1.4   \
-y
```

#### Get Validator Info

```
arkeod status 2>&1 | jq .ValidatorInfo
```

#### Get Syncing Block

```bash
arkeod status 2>&1 | jq .SyncInfo
```

#### Get Peer Own Node

```bash
echo $(arkeod tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.arkeo/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

#### Get Peer Node

```bash
curl -sS http://localhost:16701/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

#### Unjail Validator
```bash
arkeod tx slashing unjail \
--from mywallet \
--chain-id arkeo \
 --home ${HOME}/.arkeo\
 --node  http://localhost:26700 \
   --gas=auto --gas-prices=0.1uarkeo --gas-adjustment=1.4   \
 -y
```

#### Jail Reason

```bash
arkeod query slashing signing-info $(arkeod tendermint show-validator) \
 --node  http://localhost:26700 \
 --home ${HOME}/.arkeo
```

#### List All Active Validator

```bash
arkeod q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### List All Inactive Validator

```bash
arkeod q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### Show Validator Details

```
arkeod \
query staking validator \
$(arkeod keys show \ 
                $(arkeod keys list --home ${HOME}/.arkeo--output json| jq -r ".[] .address" | tail -n1) \
--bech val -a) \
--chain-id arkeo \
--node http://localhost:26700
```

### Token Management

#### Withdraw All Reward From Validator

```bash
arkeod tx distribution withdraw-all-rewards \
--from mywallet \
--chain-id arkeo \
--node http://localhost:26700 \
--home ${HOME}/.arkeo\
  --gas=auto --gas-prices=0.1uarkeo --gas-adjustment=1.4   \
-y
```

#### Withdraw Commision Reward From Validator

```bash
arkeod tx distribution withdraw-rewards $(arkeod keys show mywallet --bech val -a) \
--commission \
--from mywallet \
--chain-id arkeo \
--node http://localhost:26700 \
--home ${HOME}/.arkeo\
  --gas=auto --gas-prices=0.1uarkeo --gas-adjustment=1.4   \
-y 
```

#### Delegate My Token to Own Validator

```bash
arkeod tx staking delegate $(arkeod keys show wallet --bech val -a) 100000uarkeo \
--from mywallet \
--home ${HOME}/.arkeo\
--node http://localhost:26700 \
--chain-id arkeo \
  --gas=auto --gas-prices=0.1uarkeo --gas-adjustment=1.4   \
-y
```

#### Delegate Your Token To Our Validator

```bash
arkeod tx staking delegate prefixVALOPExxxxxx 100000uarkeo \ 
--from mywallet \
--home ${HOME}/.arkeo\
--node http://localhost:26700 \
--chain-id arkeo \
  --gas=auto --gas-prices=0.1uarkeo --gas-adjustment=1.4   \
-y
```

#### Redelegate Tokens to Another Validator

```bash
arkeod tx staking redelegate $(arkeod keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000uarkeo \
--from mywallet 
--home ${HOME}/.arkeo\
--node http://localhost:26700 \
--chain-id arkeo \
  --gas=auto --gas-prices=0.1uarkeo --gas-adjustment=1.4   \
-y 
```

#### Unbound or Unstake Your Tokens

```bash
arkeod tx staking unbond $(arkeod keys show wallet --bech val -a) 1000000uarkeo \
--from mywallet \
--home ${HOME}/.arkeo\
--node http://localhost:26700 \
--chain-id arkeo \
  --gas=auto --gas-prices=0.1uarkeo --gas-adjustment=1.4   \
-y 
```

Send tokens to the wallet

```
arkeod tx bank send wallet <TO_WALLET_ADDRESS> 1000000uarkeo \
--from mywallet \
--home ${HOME}/.arkeo\
--node http://localhost:26700 \
--chain-id arkeo \
  --gas=auto --gas-prices=0.1uarkeo --gas-adjustment=1.4   \
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
arkeod query gov proposals
```

#### How to Vote

```bash
### vote yes
arkeod tx gov vote 1 yes \
--from mywallet \
--home ${HOME}/.arkeo\
--node http://localhost:26700 \
--chain-id arkeo \
  --gas=auto --gas-prices=0.1uarkeo --gas-adjustment=1.4   \
-y 

### vote no
arkeod tx gov vote 1 no \
--from mywallet \
--home ${HOME}/.arkeo\
--node http://localhost:26700 \
--chain-id arkeo \
  --gas=auto --gas-prices=0.1uarkeo --gas-adjustment=1.4   \
-y 

### vote abstain
arkeod tx gov vote 1 abstain \
--from mywallet \
--home ${HOME}/.arkeo\
--node http://localhost:26700 \
--chain-id arkeo \
  --gas=auto --gas-prices=0.1uarkeo --gas-adjustment=1.4   \
-y 

### vote No With Veto
arkeod tx gov vote 1 nowithveto \
--from mywallet \
--home ${HOME}/.arkeo\
--node http://localhost:26700 \
--chain-id arkeo \
  --gas=auto --gas-prices=0.1uarkeo --gas-adjustment=1.4   \
-y 
```
