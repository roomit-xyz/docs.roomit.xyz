---
description: This is command about CLI Empe
---

# Command Empe

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
--gas=auto --gas-prices=0.025uempe --gas-adjustment=1.56
```
#### **Add New Key**

```bash
emped keys add mywallet --home ${HOME}/.empe-chain 
```

#### **Recover Key**

With Passphrase

```bash
emped keys add mywallet --recover  --home ${HOME}/.empe-chain       
```

With Keyring

```bash
emped keys add mywallet --recover --keyring-backend os --home ${HOME}/.empe-chain       
```

#### **List Key**

```bash
emped keys list --home ${HOME}/.empe-chain    
```

#### **Delete Key**

```bash
emped keys delete mywallet --home ${HOME}/.empe-chain
```

**Export Key**

```bash
 emped keys export mywallet --home ${HOME}/.empe-chain
```

#### Import Key

```
emped keys import mywallet mywallet_file.backup --home ${HOME}/.empe-chain
```

#### Show All Balances Address

<pre class="language-bash"><code class="lang-bash"><strong>for mywallet in `emped keys list --home ${HOME}/.empe-chain--output json| jq -r ".[] .address"`
</strong>do
   CHAIN_ID="empe-testnet-2"
   RPC="http://localhost:16710"
   emped q bank balances ${mywallet} --home ${HOME}/.empe-chain --chain-id ${CHAIN_ID} --node ${RPC}
done
</code></pre>

#### Show Balance Address

```bash
emped q bank balances mywallet_public_address --home ${HOME}/.empe-chain --chain-id empe-testnet-2 --node http://localhost:16710
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

emped tx staking create-validator \
--amount=1000000uempe \
--pubkey=$(emped tendermint show-validator --home ${HOME}/.empe-chain) \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}" \
--chain-id=empe-testnet-2 \
--commission-rate=0.05 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1 \
--from=mywallet \
--node http://localhost:16710 \
--home ${HOME}/.empe-chain\
  --gas=auto --gas-prices=0.025uempe --gas-adjustment=1.56   \
-y
```

#### Edit Validator Identities Informations

```bash
MONIKER="NAME_OF_YOUR_VALIDATOR"
PROFILE="PGP_KEY_OF_KEYBASE"
DETAILS="Describes Your Validator"
WEBSITE="https://yourwebsite.com"

emped tx staking edit-validator \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}" \
--chain-id=empe-testnet-2 \
--commission-rate=0.05 \
--from=mywallet \
--node http://localhost:16710 \
--home ${HOME}/.empe-chain \
  --gas=auto --gas-prices=0.025uempe --gas-adjustment=1.56   \
-y
```

#### Get Validator Info

```
emped status 2>&1 | jq .ValidatorInfo
```

#### Get Syncing Block

```bash
emped status 2>&1 | jq .SyncInfo
```

#### Get Peer Own Node

```bash
echo $(emped tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.empe-chain/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

#### Get Peer Node

```bash
curl -sS http://localhost:16701/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

#### Unjail Validator
```bash
emped tx slashing unjail \
--from mywallet \
--chain-id empe-testnet-2 \
 --home ${HOME}/.empe-chain\
 --node  http://localhost:16710 \
   --gas=auto --gas-prices=0.025uempe --gas-adjustment=1.56   \
 -y
```

#### Jail Reason

```bash
emped query slashing signing-info $(emped tendermint show-validator) \
 --node  http://localhost:16710 \
 --home ${HOME}/.empe-chain
```

#### List All Active Validator

```bash
emped q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### List All Inactive Validator

```bash
emped q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### Show Validator Details

```
emped \
query staking validator \
$(emped keys show \ 
                $(emped keys list --home ${HOME}/.empe-chain--output json| jq -r ".[] .address" | tail -n1) \
--bech val -a) \
--chain-id empe-testnet-2 \
--node http://localhost:16710
```

### Token Management

#### Withdraw All Reward From Validator

```bash
emped tx distribution withdraw-all-rewards \
--from mywallet \
--chain-id empe-testnet-2 \
--node http://localhost:16710 \
--home ${HOME}/.empe-chain\
  --gas=auto --gas-prices=0.025uempe --gas-adjustment=1.56   \
-y
```

#### Withdraw Commision Reward From Validator

```bash
emped tx distribution withdraw-rewards $(emped keys show mywallet --bech val -a) \
--commission \
--from mywallet \
--chain-id empe-testnet-2 \
--node http://localhost:16710 \
--home ${HOME}/.empe-chain\
  --gas=auto --gas-prices=0.025uempe --gas-adjustment=1.56   \
-y 
```

#### Delegate My Token to Own Validator

```bash
emped tx staking delegate $(emped keys show wallet --bech val -a) 100000uempe \
--from mywallet \
--home ${HOME}/.empe-chain\
--node http://localhost:16710 \
--chain-id empe-testnet-2 \
  --gas=auto --gas-prices=0.025uempe --gas-adjustment=1.56   \
-y
```

#### Delegate Your Token To Our Validator

```bash
emped tx staking delegate prefixVALOPExxxxxx 100000uempe \ 
--from mywallet \
--home ${HOME}/.empe-chain\
--node http://localhost:16710 \
--chain-id empe-testnet-2 \
  --gas=auto --gas-prices=0.025uempe --gas-adjustment=1.56   \
-y
```

#### Redelegate Tokens to Another Validator

```bash
emped tx staking redelegate $(emped keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000uempe \
--from mywallet 
--home ${HOME}/.empe-chain\
--node http://localhost:16710 \
--chain-id empe-testnet-2 \
  --gas=auto --gas-prices=0.025uempe --gas-adjustment=1.56   \
-y 
```

#### Unbound or Unstake Your Tokens

```bash
emped tx staking unbond $(emped keys show wallet --bech val -a) 1000000uempe \
--from mywallet \
--home ${HOME}/.empe-chain\
--node http://localhost:16710 \
--chain-id empe-testnet-2 \
  --gas=auto --gas-prices=0.025uempe --gas-adjustment=1.56   \
-y 
```

Send tokens to the wallet

```
emped tx bank send wallet <TO_WALLET_ADDRESS> 1000000uempe \
--from mywallet \
--home ${HOME}/.empe-chain\
--node http://localhost:16710 \
--chain-id empe-testnet-2 \
  --gas=auto --gas-prices=0.025uempe --gas-adjustment=1.56   \
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
emped query gov proposals
```

#### How to Vote

```bash
### vote yes
emped tx gov vote 1 yes \
--from mywallet \
--home ${HOME}/.empe-chain\
--node http://localhost:16710 \
--chain-id empe-testnet-2 \
  --gas=auto --gas-prices=0.025uempe --gas-adjustment=1.56   \
-y 

### vote no
emped tx gov vote 1 no \
--from mywallet \
--home ${HOME}/.empe-chain\
--node http://localhost:16710 \
--chain-id empe-testnet-2 \
  --gas=auto --gas-prices=0.025uempe --gas-adjustment=1.56   \
-y 

### vote abstain
emped tx gov vote 1 abstain \
--from mywallet \
--home ${HOME}/.empe-chain\
--node http://localhost:16710 \
--chain-id empe-testnet-2 \
  --gas=auto --gas-prices=0.025uempe --gas-adjustment=1.56   \
-y 

### vote No With Veto
emped tx gov vote 1 nowithveto \
--from mywallet \
--home ${HOME}/.empe-chain\
--node http://localhost:16710 \
--chain-id empe-testnet-2 \
  --gas=auto --gas-prices=0.025uempe --gas-adjustment=1.56   \
-y 
```
