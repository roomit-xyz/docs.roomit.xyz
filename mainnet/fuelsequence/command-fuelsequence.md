---
description: This is command about CLI Fuelsequence
---

# Command Fuelsequence

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
--gas=auto --gas-prices=10fuel --gas-adjustment=1.5
```
#### **Add New Key**

```bash
fuelsequencerd keys add mywallet --home ${HOME}/.fuelsequencer 
```

#### **Recover Key**

With Passphrase

```bash
fuelsequencerd keys add mywallet --recover  --home ${HOME}/.fuelsequencer       
```

With Keyring

```bash
fuelsequencerd keys add mywallet --recover --keyring-backend os --home ${HOME}/.fuelsequencer       
```

#### **List Key**

```bash
fuelsequencerd keys list --home ${HOME}/.fuelsequencer    
```

#### **Delete Key**

```bash
fuelsequencerd keys delete mywallet --home ${HOME}/.fuelsequencer
```

**Export Key**

```bash
 fuelsequencerd keys export mywallet --home ${HOME}/.fuelsequencer
```

#### Import Key

```
fuelsequencerd keys import mywallet mywallet_file.backup --home ${HOME}/.fuelsequencer
```

#### Show All Balances Address

<pre class="language-bash"><code class="lang-bash"><strong>for mywallet in `fuelsequencerd keys list --home ${HOME}/.fuelsequencer--output json| jq -r ".[] .address"`
</strong>do
   CHAIN_ID="seq-mainnet-1"
   RPC="http://localhost:16712"
   fuelsequencerd q bank balances ${mywallet} --home ${HOME}/.fuelsequencer --chain-id ${CHAIN_ID} --node ${RPC}
done
</code></pre>

#### Show Balance Address

```bash
fuelsequencerd q bank balances mywallet_public_address --home ${HOME}/.fuelsequencer --chain-id seq-mainnet-1 --node http://localhost:16712
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

fuelsequencerd tx staking create-validator \
--amount=1000000000fuel \
--pubkey=$(fuelsequencerd tendermint show-validator --home ${HOME}/.fuelsequencer) \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}" \
--chain-id=seq-mainnet-1 \
--commission-rate=0.05 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1 \
--from=mywallet \
--node http://localhost:16712 \
--home ${HOME}/.fuelsequencer\
  --gas=auto --gas-prices=10fuel --gas-adjustment=1.5   \
-y
```

#### Edit Validator Identities Informations

```bash
MONIKER="NAME_OF_YOUR_VALIDATOR"
PROFILE="PGP_KEY_OF_KEYBASE"
DETAILS="Describes Your Validator"
WEBSITE="https://yourwebsite.com"

fuelsequencerd tx staking edit-validator \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}" \
--chain-id=seq-mainnet-1 \
--commission-rate=0.05 \
--from=mywallet \
--node http://localhost:16712 \
--home ${HOME}/.fuelsequencer \
  --gas=auto --gas-prices=10fuel --gas-adjustment=1.5   \
-y
```

#### Get Validator Info

```
fuelsequencerd status 2>&1 | jq .ValidatorInfo
```

#### Get Syncing Block

```bash
fuelsequencerd status 2>&1 | jq .SyncInfo
```

#### Get Peer Own Node

```bash
echo $(fuelsequencerd tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.fuelsequencer/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

#### Get Peer Node

```bash
curl -sS http://localhost:16701/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

#### Unjail Validator
```bash
fuelsequencerd tx slashing unjail \
--from mywallet \
--chain-id seq-mainnet-1 \
 --home ${HOME}/.fuelsequencer\
 --node  http://localhost:16712 \
   --gas=auto --gas-prices=10fuel --gas-adjustment=1.5   \
 -y
```

#### Jail Reason

```bash
fuelsequencerd query slashing signing-info $(fuelsequencerd tendermint show-validator) \
 --node  http://localhost:16712 \
 --home ${HOME}/.fuelsequencer
```

#### List All Active Validator

```bash
fuelsequencerd q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### List All Inactive Validator

```bash
fuelsequencerd q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### Show Validator Details

```
fuelsequencerd \
query staking validator \
$(fuelsequencerd keys show \ 
                $(fuelsequencerd keys list --home ${HOME}/.fuelsequencer--output json| jq -r ".[] .address" | tail -n1) \
--bech val -a) \
--chain-id seq-mainnet-1 \
--node http://localhost:16712
```

### Token Management

#### Withdraw All Reward From Validator

```bash
fuelsequencerd tx distribution withdraw-all-rewards \
--from mywallet \
--chain-id seq-mainnet-1 \
--node http://localhost:16712 \
--home ${HOME}/.fuelsequencer\
  --gas=auto --gas-prices=10fuel --gas-adjustment=1.5   \
-y
```

#### Withdraw Commision Reward From Validator

```bash
fuelsequencerd tx distribution withdraw-rewards $(fuelsequencerd keys show mywallet --bech val -a) \
--commission \
--from mywallet \
--chain-id seq-mainnet-1 \
--node http://localhost:16712 \
--home ${HOME}/.fuelsequencer\
  --gas=auto --gas-prices=10fuel --gas-adjustment=1.5   \
-y 
```

#### Delegate My Token to Own Validator

```bash
fuelsequencerd tx staking delegate $(fuelsequencerd keys show wallet --bech val -a) 100000fuel \
--from mywallet \
--home ${HOME}/.fuelsequencer\
--node http://localhost:16712 \
--chain-id seq-mainnet-1 \
  --gas=auto --gas-prices=10fuel --gas-adjustment=1.5   \
-y
```

#### Delegate Your Token To Our Validator

```bash
fuelsequencerd tx staking delegate prefixVALOPExxxxxx 100000fuel \ 
--from mywallet \
--home ${HOME}/.fuelsequencer\
--node http://localhost:16712 \
--chain-id seq-mainnet-1 \
  --gas=auto --gas-prices=10fuel --gas-adjustment=1.5   \
-y
```

#### Redelegate Tokens to Another Validator

```bash
fuelsequencerd tx staking redelegate $(fuelsequencerd keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000fuel \
--from mywallet 
--home ${HOME}/.fuelsequencer\
--node http://localhost:16712 \
--chain-id seq-mainnet-1 \
  --gas=auto --gas-prices=10fuel --gas-adjustment=1.5   \
-y 
```

#### Unbound or Unstake Your Tokens

```bash
fuelsequencerd tx staking unbond $(fuelsequencerd keys show wallet --bech val -a) 1000000fuel \
--from mywallet \
--home ${HOME}/.fuelsequencer\
--node http://localhost:16712 \
--chain-id seq-mainnet-1 \
  --gas=auto --gas-prices=10fuel --gas-adjustment=1.5   \
-y 
```

Send tokens to the wallet

```
fuelsequencerd tx bank send wallet <TO_WALLET_ADDRESS> 1000000fuel \
--from mywallet \
--home ${HOME}/.fuelsequencer\
--node http://localhost:16712 \
--chain-id seq-mainnet-1 \
  --gas=auto --gas-prices=10fuel --gas-adjustment=1.5   \
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
fuelsequencerd query gov proposals
```

#### How to Vote

```bash
### vote yes
fuelsequencerd tx gov vote 1 yes \
--from mywallet \
--home ${HOME}/.fuelsequencer\
--node http://localhost:16712 \
--chain-id seq-mainnet-1 \
  --gas=auto --gas-prices=10fuel --gas-adjustment=1.5   \
-y 

### vote no
fuelsequencerd tx gov vote 1 no \
--from mywallet \
--home ${HOME}/.fuelsequencer\
--node http://localhost:16712 \
--chain-id seq-mainnet-1 \
  --gas=auto --gas-prices=10fuel --gas-adjustment=1.5   \
-y 

### vote abstain
fuelsequencerd tx gov vote 1 abstain \
--from mywallet \
--home ${HOME}/.fuelsequencer\
--node http://localhost:16712 \
--chain-id seq-mainnet-1 \
  --gas=auto --gas-prices=10fuel --gas-adjustment=1.5   \
-y 

### vote No With Veto
fuelsequencerd tx gov vote 1 nowithveto \
--from mywallet \
--home ${HOME}/.fuelsequencer\
--node http://localhost:16712 \
--chain-id seq-mainnet-1 \
  --gas=auto --gas-prices=10fuel --gas-adjustment=1.5   \
-y 
```
