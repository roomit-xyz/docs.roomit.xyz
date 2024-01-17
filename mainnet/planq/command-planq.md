# Command PlanQ

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

We never use this username in our production!
{% endhint %}

#### **Add New Key**

```bash
planqd keys add mywallet --home ${HOME}/.planqd      
```

#### **Recover Key**

With Passphrase

```bash
planqd keys add mywallet --recover  --home ${HOME}/.planqd            
```

With Keyring

```bash
planqd keys add mywallet --recover --keyring-backend os --home ${HOME}/.planqd            
```

#### **List Key**

```bash
planqd keys list --home ${HOME}/.planqd         
```

#### **Delete Key**

```bash
planqd keys delete mywallet --home ${HOME}/.planqd  
```

**Export Key**

```bash
 planqd keys export mywallet --home ${HOME}/.planqd  
```

#### Import Key

```
planqd keys import mywallet mywallet_file.backup --home ${HOME}/.planqd 
```

#### Show All Balances Address

<pre class="language-bash"><code class="lang-bash"><strong>for mywallet in `planqd keys list --home ${HOME}/.planqd --output json| jq -r ".[] .address"`
</strong>do
   CHAIN_ID="planq_7070-2"
   RPC="tcp://localhost:16703"
   planqd q bank balances ${mywallet} --home ${HOME}/.planqd --chain-id ${CHAIN_ID} --node ${RPC}
done
</code></pre>

#### Show Balance Address

```bash
planqd q bank balances mywallet_public_address --home ${HOME}/.planqd --chain-id planq_7070-2 --node tcp://localhost:16703
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

planqd tx staking create-validator \
--amount=1000000ucmdx \
--pubkey=$(planqd tendermint show-validator --home ${HOME}/.planqd) \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}" \
--chain-id=planq_7070-2 \
--commission-rate=0.05 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1 \
--from=mywallet \
--gas-adjustment=1.4 \
--gas=auto \
--node tcp://localhost:16703 \
--home ${HOME}/.planqd \
-y
```

#### Edit Validator Identities Informations

```bash
MONIKER="NAME_OF_YOUR_VALIDATOR"
PROFILE="PGP_KEY_OF_KEYBASE"
DETAILS="Describes Your Validator"
WEBSITE="https://yourwebsite.com"

planqd tx staking edit-validator \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}" \
--chain-id=planq_7070-2 \
--commission-rate=0.05 \
--from=mywallet \
--gas-adjustment=1.4 \
--gas=auto \
--node tcp://localhost:16703 \
--home ${HOME}/.planqd \
-y
```

#### Get Validator Info

```
planqd status 2>&1 | jq .ValidatorInfo
```

#### Get Syncing Block

```
planqd status 2>&1 | jq .SyncInfo
```

#### Get Peer Own Node

```bash
echo $(planqd tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.planqd/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

#### Get Peer Node

```
curl -sS http://localhost:16703/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

#### Unjail Validator

```bash
planqd tx slashing unjail \
--from mywallet \
--chain-id planq_7070-2 \
 --home ${HOME}/.planqd \
 --node  tcp://localhost:16703 \
 --gas auto \
 --gas-adjustment 1.4 \
 -y
```

#### Jail Reason

```bash
planqd query slashing signing-info $(planqd tendermint show-validator) \
 --node  tcp://localhost:16703 \
 --home ${HOME}/.planqd
```

#### List All Active Validator

```bash
planqd q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### List All Inactive Validator

```bash
planqd q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### Show Validator Details

```
planqd \
query staking validator \
$(planqd keys show \ 
                $(planqd keys list --home ${HOME}/.planqd --output json| jq -r ".[] .address" | tail -n1) \
--bech val -a) \
--chain-id planq_7070-2 \
--node tcp://localhost:16703
```

### Token Management

#### Withdraw All Reward From Validator

```bash
planqd tx distribution withdraw-all-rewards \
--from mywallet \
--chain-id planq_7070-2 \
--node tcp://localhost:16703 \
--home ${HOME}/.planqd \
--gas-adjustment 1.4 \
--gas auto \
-y
```

#### Withdraw Commision Reward From Validator

```bash
planqd tx distribution withdraw-rewards $(planqd keys show mywallet --bech val -a) \
--commission \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--chain-id planq_7070-2 \
--node tcp://localhost:16703 \
--home ${HOME}/.planqd \
-y 
```

#### Delegate My Token to Own Validator

```bash
planqd tx staking delegate $(planqd keys show wallet --bech val -a) 1000000aplanq \
--from mywallet \
--gas-adjustment 1.4 \
--home ${HOME}/.planqd \
--node tcp://localhost:16703 \
--chain-id planq_7070-2 \
--gas auto \
-y
```

#### Delegate Your Token To Our Validator

```bash
planqd tx staking delegate plqvaloper1fqnr328nlndkxek2jaz8teec0euyr5yh26q26l 1000000aplanq \ 
--from mywallet \
--gas-adjustment 1.4 \ 
--gas auto \
--home ${HOME}/.planqd \
--node tcp://localhost:16703 \
--chain-id planq_7070-2 \
-y
```

#### Redelegate Tokens to Another Validator

```bash
planqd tx staking redelegate $(planqd keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 1000000aplanq \
--from mywallet 
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.planqd \
--node tcp://localhost:16703 \
--chain-id planq_7070-2 \
-y 
```

#### Unbound or Unstake Your Tokens

```bash
planqd tx staking unbond $(planqd keys show wallet --bech val -a) 1000000aplanq \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.planqd \
--node tcp://localhost:16703 \
--chain-id planq_7070-2 \
-y 
```

Send tokens to the wallet

```
planqd tx bank send wallet <TO_WALLET_ADDRESS> 1000000aplanq \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.planqd \
--node tcp://localhost:16703 \
--chain-id planq_7070-2 \
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
planqd query gov proposals
```

#### How to Vote

```bash
### vote yes
planqd tx gov vote 1 yes \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.planqd \
--node tcp://localhost:16703 \
--chain-id planq_7070-2 \
-y 

### vote no
planqd tx gov vote 1 no \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.planqd \
--node tcp://localhost:16703 \
--chain-id planq_7070-2 \
-y 

### vote abstain
planqd tx gov vote 1 abstain \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.planqd \
--node tcp://localhost:16703 \
--chain-id planq_7070-2 \
-y 

### vote No With Veto
planqd tx gov vote 1 nowithveto \
--from mywallet \
--gas-adjustment 1.4 \
--gas auto \
--home ${HOME}/.planqd \
--node tcp://localhost:16703 \
--chain-id planq_7070-2 \
-y 
```
