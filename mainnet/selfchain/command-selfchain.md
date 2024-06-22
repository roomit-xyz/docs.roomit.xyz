---
description: This is command about CLI Selfchain
---

# Command Selfchain

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
--gas=auto --gas-prices=0.5uslf --gas-adjustment=1.2
```
#### **Add New Key**

```bash
selfchaind keys add mywallet --home ${HOME}/.selfchain 
```

#### **Recover Key**

With Passphrase

```bash
selfchaind keys add mywallet --recover  --home ${HOME}/.selfchain       
```

With Keyring

```bash
selfchaind keys add mywallet --recover --keyring-backend os --home ${HOME}/.selfchain       
```

#### **List Key**

```bash
selfchaind keys list --home ${HOME}/.selfchain    
```

#### **Delete Key**

```bash
selfchaind keys delete mywallet --home ${HOME}/.selfchain
```

**Export Key**

```bash
 selfchaind keys export mywallet --home ${HOME}/.selfchain
```

#### Import Key

```
selfchaind keys import mywallet mywallet_file.backup --home ${HOME}/.selfchain
```

#### Show All Balances Address

<pre class="language-bash"><code class="lang-bash"><strong>for mywallet in `selfchaind keys list --home ${HOME}/.selfchain--output json| jq -r ".[] .address"`
</strong>do
   CHAIN_ID="self-1"
   RPC="http://localhost:16709"
   selfchaind q bank balances ${mywallet} --home ${HOME}/.selfchain --chain-id ${CHAIN_ID} --node ${RPC}
done
</code></pre>

#### Show Balance Address

```bash
selfchaind q bank balances mywallet_public_address --home ${HOME}/.selfchain --chain-id self-1 --node http://localhost:16709
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

selfchaind tx staking create-validator \
--amount=1000000uslf \
--pubkey=$(selfchaind tendermint show-validator --home ${HOME}/.selfchain) \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}" \
--chain-id=self-1 \
--commission-rate=0.05 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1 \
--from=mywallet \
--node http://localhost:16709 \
--home ${HOME}/.selfchain\
--gas=auto --gas-prices=0.5uslf --gas-adjustment=1.2 \
-y
```

#### Edit Validator Identities Informations

```bash
MONIKER="NAME_OF_YOUR_VALIDATOR"
PROFILE="PGP_KEY_OF_KEYBASE"
DETAILS="Describes Your Validator"
WEBSITE="https://yourwebsite.com"

selfchaind tx staking edit-validator \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}" \
--chain-id=self-1 \
--commission-rate=0.05 \
--from=mywallet \
--node http://localhost:16709 \
--home ${HOME}/.selfchain \
--gas=auto --gas-prices=0.5uslf --gas-adjustment=1.2 \
-y
```

#### Get Validator Info

```
selfchaind status 2>&1 | jq .ValidatorInfo
```

#### Get Syncing Block

```bash
selfchaind status 2>&1 | jq .SyncInfo
```

#### Get Peer Own Node

```bash
echo $(selfchaind tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.selfchain/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

#### Get Peer Node

```bash
curl -sS http://localhost:16701/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

#### Unjail Validator
```bash
selfchaind tx slashing unjail \
--from mywallet \
--chain-id self-1 \
 --home ${HOME}/.selfchain\
 --node  http://localhost:16709 \
 --gas=auto --gas-prices=0.5uslf --gas-adjustment=1.2 \
 -y
```

#### Jail Reason

```bash
selfchaind query slashing signing-info $(selfchaind tendermint show-validator) \
 --node  http://localhost:16709 \
 --home ${HOME}/.selfchain
```

#### List All Active Validator

```bash
selfchaind q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### List All Inactive Validator

```bash
selfchaind q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### Show Validator Details

```
selfchaind \
query staking validator \
$(selfchaind keys show \ 
                $(selfchaind keys list --home ${HOME}/.selfchain--output json| jq -r ".[] .address" | tail -n1) \
--bech val -a) \
--chain-id self-1 \
--node http://localhost:16709
```

### Token Management

#### Withdraw All Reward From Validator

```bash
selfchaind tx distribution withdraw-all-rewards \
--from mywallet \
--chain-id self-1 \
--node http://localhost:16709 \
--home ${HOME}/.selfchain\
--gas=auto --gas-prices=0.5uslf --gas-adjustment=1.2 \
-y
```

#### Withdraw Commision Reward From Validator

```bash
selfchaind tx distribution withdraw-rewards $(selfchaind keys show mywallet --bech val -a) \
--commission \
--from mywallet \
--chain-id self-1 \
--node http://localhost:16709 \
--home ${HOME}/.selfchain\
--gas=auto --gas-prices=0.5uslf --gas-adjustment=1.2 \
-y 
```

#### Delegate My Token to Own Validator

```bash
selfchaind tx staking delegate $(selfchaind keys show wallet --bech val -a) 100000uslf \
--from mywallet \
--home ${HOME}/.selfchain\
--node http://localhost:16709 \
--chain-id self-1 \
--gas=auto --gas-prices=0.5uslf --gas-adjustment=1.2 \
-y
```

#### Delegate Your Token To Our Validator

```bash
selfchaind tx staking delegate prefixVALOPExxxxxx 100000uslf \ 
--from mywallet \
--home ${HOME}/.selfchain\
--node http://localhost:16709 \
--chain-id self-1 \
--gas=auto --gas-prices=0.5uslf --gas-adjustment=1.2 \
-y
```

#### Redelegate Tokens to Another Validator

```bash
selfchaind tx staking redelegate $(selfchaind keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000uslf \
--from mywallet 
--home ${HOME}/.selfchain\
--node http://localhost:16709 \
--chain-id self-1 \
--gas=auto --gas-prices=0.5uslf --gas-adjustment=1.2 \
-y 
```

#### Unbound or Unstake Your Tokens

```bash
selfchaind tx staking unbond $(selfchaind keys show wallet --bech val -a) 1000000uslf \
--from mywallet \
--home ${HOME}/.selfchain\
--node http://localhost:16709 \
--chain-id self-1 \
--gas=auto --gas-prices=0.5uslf --gas-adjustment=1.2 \
-y 
```

Send tokens to the wallet

```
selfchaind tx bank send wallet <TO_WALLET_ADDRESS> 1000000uslf \
--from mywallet \
--home ${HOME}/.selfchain\
--node http://localhost:16709 \
--chain-id self-1 \
--gas=auto --gas-prices=0.5uslf --gas-adjustment=1.2 \
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
selfchaind query gov proposals
```

#### How to Vote

```bash
### vote yes
selfchaind tx gov vote 1 yes \
--from mywallet \
--home ${HOME}/.selfchain\
--node http://localhost:16709 \
--chain-id self-1 \
--gas=auto --gas-prices=0.5uslf --gas-adjustment=1.2 \
-y 

### vote no
selfchaind tx gov vote 1 no \
--from mywallet \
--home ${HOME}/.selfchain\
--node http://localhost:16709 \
--chain-id self-1 \
--gas=auto --gas-prices=0.5uslf --gas-adjustment=1.2 \
-y 

### vote abstain
selfchaind tx gov vote 1 abstain \
--from mywallet \
--home ${HOME}/.selfchain\
--node http://localhost:16709 \
--chain-id self-1 \
--gas=auto --gas-prices=0.5uslf --gas-adjustment=1.2 \
-y 

### vote No With Veto
selfchaind tx gov vote 1 nowithveto \
--from mywallet \
--home ${HOME}/.selfchain\
--node http://localhost:16709 \
--chain-id self-1 \
--gas=auto --gas-prices=0.5uslf --gas-adjustment=1.2 \
-y 
```
