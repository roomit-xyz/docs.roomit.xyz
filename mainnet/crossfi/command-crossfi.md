---
description: This is command about CLI Crossfi
---

# Command Crossfi

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
--gas=auto --gas-prices=10000000000000mpx --gas-adjustment=1.5
```
#### **Add New Key**

```bash
mineplex-chaind keys add mywallet --home ${HOME}/.mineplex-chain 
```

#### **Recover Key**

With Passphrase

```bash
mineplex-chaind keys add mywallet --recover  --home ${HOME}/.mineplex-chain       
```

With Keyring

```bash
mineplex-chaind keys add mywallet --recover --keyring-backend os --home ${HOME}/.mineplex-chain       
```

#### **List Key**

```bash
mineplex-chaind keys list --home ${HOME}/.mineplex-chain    
```

#### **Delete Key**

```bash
mineplex-chaind keys delete mywallet --home ${HOME}/.mineplex-chain
```

**Export Key**

```bash
 mineplex-chaind keys export mywallet --home ${HOME}/.mineplex-chain
```

#### Import Key

```
mineplex-chaind keys import mywallet mywallet_file.backup --home ${HOME}/.mineplex-chain
```

#### Show All Balances Address

<pre class="language-bash"><code class="lang-bash"><strong>for mywallet in `mineplex-chaind keys list --home ${HOME}/.mineplex-chain--output json| jq -r ".[] .address"`
</strong>do
   CHAIN_ID="mineplex-mainnet-1"
   RPC="http://localhost:26710"
   mineplex-chaind q bank balances ${mywallet} --home ${HOME}/.mineplex-chain --chain-id ${CHAIN_ID} --node ${RPC}
done
</code></pre>

#### Show Balance Address

```bash
mineplex-chaind q bank balances mywallet_public_address --home ${HOME}/.mineplex-chain --chain-id mineplex-mainnet-1 --node http://localhost:26710
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

mineplex-chaind tx staking create-validator \
--amount=1000000mpx \
--pubkey=$(mineplex-chaind tendermint show-validator --home ${HOME}/.mineplex-chain) \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}" \
--chain-id=mineplex-mainnet-1 \
--commission-rate=0.05 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1 \
--from=mywallet \
--node http://localhost:26710 \
--home ${HOME}/.mineplex-chain\
  --gas=auto --gas-prices=10000000000000mpx --gas-adjustment=1.5   \
-y
```

#### Edit Validator Identities Informations

```bash
MONIKER="NAME_OF_YOUR_VALIDATOR"
PROFILE="PGP_KEY_OF_KEYBASE"
DETAILS="Describes Your Validator"
WEBSITE="https://yourwebsite.com"

mineplex-chaind tx staking edit-validator \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}" \
--chain-id=mineplex-mainnet-1 \
--commission-rate=0.05 \
--from=mywallet \
--node http://localhost:26710 \
--home ${HOME}/.mineplex-chain \
  --gas=auto --gas-prices=10000000000000mpx --gas-adjustment=1.5   \
-y
```

#### Get Validator Info

```
mineplex-chaind status 2>&1 | jq .ValidatorInfo
```

#### Get Syncing Block

```bash
mineplex-chaind status 2>&1 | jq .SyncInfo
```

#### Get Peer Own Node

```bash
echo $(mineplex-chaind tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.mineplex-chain/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

#### Get Peer Node

```bash
curl -sS http://localhost:16701/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

#### Unjail Validator
```bash
mineplex-chaind tx slashing unjail \
--from mywallet \
--chain-id mineplex-mainnet-1 \
 --home ${HOME}/.mineplex-chain\
 --node  http://localhost:26710 \
   --gas=auto --gas-prices=10000000000000mpx --gas-adjustment=1.5   \
 -y
```

#### Jail Reason

```bash
mineplex-chaind query slashing signing-info $(mineplex-chaind tendermint show-validator) \
 --node  http://localhost:26710 \
 --home ${HOME}/.mineplex-chain
```

#### List All Active Validator

```bash
mineplex-chaind q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### List All Inactive Validator

```bash
mineplex-chaind q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### Show Validator Details

```
mineplex-chaind \
query staking validator \
$(mineplex-chaind keys show \ 
                $(mineplex-chaind keys list --home ${HOME}/.mineplex-chain--output json| jq -r ".[] .address" | tail -n1) \
--bech val -a) \
--chain-id mineplex-mainnet-1 \
--node http://localhost:26710
```

### Token Management

#### Withdraw All Reward From Validator

```bash
mineplex-chaind tx distribution withdraw-all-rewards \
--from mywallet \
--chain-id mineplex-mainnet-1 \
--node http://localhost:26710 \
--home ${HOME}/.mineplex-chain\
  --gas=auto --gas-prices=10000000000000mpx --gas-adjustment=1.5   \
-y
```

#### Withdraw Commision Reward From Validator

```bash
mineplex-chaind tx distribution withdraw-rewards $(mineplex-chaind keys show mywallet --bech val -a) \
--commission \
--from mywallet \
--chain-id mineplex-mainnet-1 \
--node http://localhost:26710 \
--home ${HOME}/.mineplex-chain\
  --gas=auto --gas-prices=10000000000000mpx --gas-adjustment=1.5   \
-y 
```

#### Delegate My Token to Own Validator

```bash
mineplex-chaind tx staking delegate $(mineplex-chaind keys show wallet --bech val -a) 100000mpx \
--from mywallet \
--home ${HOME}/.mineplex-chain\
--node http://localhost:26710 \
--chain-id mineplex-mainnet-1 \
  --gas=auto --gas-prices=10000000000000mpx --gas-adjustment=1.5   \
-y
```

#### Delegate Your Token To Our Validator

```bash
mineplex-chaind tx staking delegate prefixVALOPExxxxxx 100000mpx \ 
--from mywallet \
--home ${HOME}/.mineplex-chain\
--node http://localhost:26710 \
--chain-id mineplex-mainnet-1 \
  --gas=auto --gas-prices=10000000000000mpx --gas-adjustment=1.5   \
-y
```

#### Redelegate Tokens to Another Validator

```bash
mineplex-chaind tx staking redelegate $(mineplex-chaind keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000mpx \
--from mywallet 
--home ${HOME}/.mineplex-chain\
--node http://localhost:26710 \
--chain-id mineplex-mainnet-1 \
  --gas=auto --gas-prices=10000000000000mpx --gas-adjustment=1.5   \
-y 
```

#### Unbound or Unstake Your Tokens

```bash
mineplex-chaind tx staking unbond $(mineplex-chaind keys show wallet --bech val -a) 1000000mpx \
--from mywallet \
--home ${HOME}/.mineplex-chain\
--node http://localhost:26710 \
--chain-id mineplex-mainnet-1 \
  --gas=auto --gas-prices=10000000000000mpx --gas-adjustment=1.5   \
-y 
```

Send tokens to the wallet

```
mineplex-chaind tx bank send wallet <TO_WALLET_ADDRESS> 1000000mpx \
--from mywallet \
--home ${HOME}/.mineplex-chain\
--node http://localhost:26710 \
--chain-id mineplex-mainnet-1 \
  --gas=auto --gas-prices=10000000000000mpx --gas-adjustment=1.5   \
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
mineplex-chaind query gov proposals
```

#### How to Vote

```bash
### vote yes
mineplex-chaind tx gov vote 1 yes \
--from mywallet \
--home ${HOME}/.mineplex-chain\
--node http://localhost:26710 \
--chain-id mineplex-mainnet-1 \
  --gas=auto --gas-prices=10000000000000mpx --gas-adjustment=1.5   \
-y 

### vote no
mineplex-chaind tx gov vote 1 no \
--from mywallet \
--home ${HOME}/.mineplex-chain\
--node http://localhost:26710 \
--chain-id mineplex-mainnet-1 \
  --gas=auto --gas-prices=10000000000000mpx --gas-adjustment=1.5   \
-y 

### vote abstain
mineplex-chaind tx gov vote 1 abstain \
--from mywallet \
--home ${HOME}/.mineplex-chain\
--node http://localhost:26710 \
--chain-id mineplex-mainnet-1 \
  --gas=auto --gas-prices=10000000000000mpx --gas-adjustment=1.5   \
-y 

### vote No With Veto
mineplex-chaind tx gov vote 1 nowithveto \
--from mywallet \
--home ${HOME}/.mineplex-chain\
--node http://localhost:26710 \
--chain-id mineplex-mainnet-1 \
  --gas=auto --gas-prices=10000000000000mpx --gas-adjustment=1.5   \
-y 
```
