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

#### Gas Fee
        
```
--gas=auto --gas-prices=10000000000000mpx --gas-adjustment=1.5
```
#### **Add New Key**

```bash
crossfid keys add mywallet --home ${HOME}/.crossfid 
```

#### **Recover Key**

With Passphrase

```bash
crossfid keys add mywallet --recover  --home ${HOME}/.crossfid       
```

With Keyring

```bash
crossfid keys add mywallet --recover --keyring-backend os --home ${HOME}/.crossfid       
```

#### **List Key**

```bash
crossfid keys list --home ${HOME}/.crossfid    
```

#### **Delete Key**

```bash
crossfid keys delete mywallet --home ${HOME}/.crossfid
```

**Export Key**

```bash
 crossfid keys export mywallet --home ${HOME}/.crossfid
```

#### Import Key

```
crossfid keys import mywallet mywallet_file.backup --home ${HOME}/.crossfid
```

#### Show All Balances Address

<pre class="language-bash"><code class="lang-bash"><strong>for mywallet in `crossfid keys list --home ${HOME}/.crossfid--output json| jq -r ".[] .address"`
</strong>do
   CHAIN_ID="crossfi-evm-testnet-1"
   RPC="http:////localhost:26705"
   crossfid q bank balances ${mywallet} --home ${HOME}/.crossfid --chain-id ${CHAIN_ID} --node ${RPC}
done
</code></pre>

#### Show Balance Address

```bash
crossfid q bank balances mywallet_public_address --home ${HOME}/.crossfid --chain-id crossfi-evm-testnet-1 --node http:////localhost:26705
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

crossfid tx staking create-validator \
--amount=1000000mpx \
--pubkey=$(crossfid tendermint show-validator --home ${HOME}/.crossfid) \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}" \
--chain-id=crossfi-evm-testnet-1 \
--commission-rate=0.05 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1 \
--from=mywallet \
--node http:////localhost:26705 \
--home ${HOME}/.crossfid\
--gas=auto --gas-prices=10000000000000mpx --gas-adjustment=1.5 \
-y
```

#### Edit Validator Identities Informations

```bash
MONIKER="NAME_OF_YOUR_VALIDATOR"
PROFILE="PGP_KEY_OF_KEYBASE"
DETAILS="Describes Your Validator"
WEBSITE="https://yourwebsite.com"

crossfid tx staking edit-validator \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}" \
--chain-id=crossfi-evm-testnet-1 \
--commission-rate=0.05 \
--from=mywallet \
--node http:////localhost:26705 \
--home ${HOME}/.crossfid \
--gas=auto --gas-prices=10000000000000mpx --gas-adjustment=1.5 \
-y
```

#### Get Validator Info

```
crossfid status 2>&1 | jq .ValidatorInfo
```

#### Get Syncing Block

```bash
crossfid status 2>&1 | jq .SyncInfo
```

#### Get Peer Own Node

```bash
echo $(crossfid tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.crossfid/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

#### Get Peer Node

```bash
curl -sS http://localhost:16701/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

#### Unjail Validator
```bash
crossfid tx slashing unjail \
--from mywallet \
--chain-id crossfi-evm-testnet-1 \
 --home ${HOME}/.crossfid\
 --node  http:////localhost:26705 \
 --gas=auto --gas-prices=10000000000000mpx --gas-adjustment=1.5 \
 -y
```

#### Jail Reason

```bash
crossfid query slashing signing-info $(crossfid tendermint show-validator) \
 --node  http:////localhost:26705 \
 --home ${HOME}/.crossfid
```

#### List All Active Validator

```bash
crossfid q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### List All Inactive Validator

```bash
crossfid q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### Show Validator Details

```
crossfid \
query staking validator \
$(crossfid keys show \ 
                $(crossfid keys list --home ${HOME}/.crossfid--output json| jq -r ".[] .address" | tail -n1) \
--bech val -a) \
--chain-id crossfi-evm-testnet-1 \
--node http:////localhost:26705
```

### Token Management

#### Withdraw All Reward From Validator

```bash
crossfid tx distribution withdraw-all-rewards \
--from mywallet \
--chain-id crossfi-evm-testnet-1 \
--node http:////localhost:26705 \
--home ${HOME}/.crossfid\
--gas=auto --gas-prices=10000000000000mpx --gas-adjustment=1.5 \
-y
```

#### Withdraw Commision Reward From Validator

```bash
crossfid tx distribution withdraw-rewards $(crossfid keys show mywallet --bech val -a) \
--commission \
--from mywallet \
--chain-id crossfi-evm-testnet-1 \
--node http:////localhost:26705 \
--home ${HOME}/.crossfid\
--gas=auto --gas-prices=10000000000000mpx --gas-adjustment=1.5 \
-y 
```

#### Delegate My Token to Own Validator

```bash
crossfid tx staking delegate $(crossfid keys show wallet --bech val -a) 100000mpx \
--from mywallet \
--home ${HOME}/.crossfid\
--node http:////localhost:26705 \
--chain-id crossfi-evm-testnet-1 \
--gas=auto --gas-prices=10000000000000mpx --gas-adjustment=1.5 \
-y
```

#### Delegate Your Token To Our Validator

```bash
crossfid tx staking delegate prefixVALOPExxxxxx 100000mpx \ 
--from mywallet \
--home ${HOME}/.crossfid\
--node http:////localhost:26705 \
--chain-id crossfi-evm-testnet-1 \
--gas=auto --gas-prices=10000000000000mpx --gas-adjustment=1.5 \
-y
```

#### Redelegate Tokens to Another Validator

```bash
crossfid tx staking redelegate $(crossfid keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000mpx \
--from mywallet 
--home ${HOME}/.crossfid\
--node http:////localhost:26705 \
--chain-id crossfi-evm-testnet-1 \
--gas=auto --gas-prices=10000000000000mpx --gas-adjustment=1.5 \
-y 
```

#### Unbound or Unstake Your Tokens

```bash
crossfid tx staking unbond $(crossfid keys show wallet --bech val -a) 1000000mpx \
--from mywallet \
--home ${HOME}/.crossfid\
--node http:////localhost:26705 \
--chain-id crossfi-evm-testnet-1 \
--gas=auto --gas-prices=10000000000000mpx --gas-adjustment=1.5 \
-y 
```

Send tokens to the wallet

```
crossfid tx bank send wallet <TO_WALLET_ADDRESS> 1000000mpx \
--from mywallet \
--home ${HOME}/.crossfid\
--node http:////localhost:26705 \
--chain-id crossfi-evm-testnet-1 \
--gas=auto --gas-prices=10000000000000mpx --gas-adjustment=1.5 \
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
crossfid query gov proposals
```

#### How to Vote

```bash
### vote yes
crossfid tx gov vote 1 yes \
--from mywallet \
--home ${HOME}/.crossfid\
--node http:////localhost:26705 \
--chain-id crossfi-evm-testnet-1 \
--gas=auto --gas-prices=10000000000000mpx --gas-adjustment=1.5 \
-y 

### vote no
crossfid tx gov vote 1 no \
--from mywallet \
--home ${HOME}/.crossfid\
--node http:////localhost:26705 \
--chain-id crossfi-evm-testnet-1 \
--gas=auto --gas-prices=10000000000000mpx --gas-adjustment=1.5 \
-y 

### vote abstain
crossfid tx gov vote 1 abstain \
--from mywallet \
--home ${HOME}/.crossfid\
--node http:////localhost:26705 \
--chain-id crossfi-evm-testnet-1 \
--gas=auto --gas-prices=10000000000000mpx --gas-adjustment=1.5 \
-y 

### vote No With Veto
crossfid tx gov vote 1 nowithveto \
--from mywallet \
--home ${HOME}/.crossfid\
--node http:////localhost:26705 \
--chain-id crossfi-evm-testnet-1 \
--gas=auto --gas-prices=10000000000000mpx --gas-adjustment=1.5 \
-y 
```
