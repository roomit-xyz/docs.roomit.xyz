# Command Blockx

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
blockd keys add mywallet --home ${HOME}/.blockd      
```

#### **Recover Key**

With Passphrase

```bash
blockd keys add mywallet --recover  --home ${HOME}/.blockd            
```

With Keyring

```bash
blockd keys add mywallet --recover --keyring-backend os --home ${HOME}/.blockd            
```

#### **List Key**

```bash
blockd keys list --home ${HOME}/.blockd         
```

#### **Delete Key**

```bash
blockd keys delete mywallet --home ${HOME}/.blockd  
```

**Export Key**

```bash
 blockd keys export mywallet --home ${HOME}/.blockd  
```

#### Import Key

```
blockd keys import mywallet mywallet_file.backup --home ${HOME}/.blockd 
```

#### Show All Balances Address

<pre class="language-bash"><code class="lang-bash"><strong>for mywallet in `blockd keys list --home ${HOME}/.blockd --output json| jq -r ".[] .address"`
</strong>do
   CHAIN_ID="blockx_100-1"
   RPC="tcp://localhost:16707"
   blockd q bank balances ${mywallet} --home ${HOME}/.blockd --chain-id ${CHAIN_ID} --node ${RPC}
done
</code></pre>

#### Show Balance Address

```bash
blockd q bank balances mywallet_public_address --home ${HOME}/.blockd --chain-id blockx_100-1 --node tcp://localhost:16707
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

blockd tx staking create-validator \
--amount=1000000ucmdx \
--pubkey=$(blockd tendermint show-validator --home ${HOME}/.blockd) \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}" \
--chain-id=blockx_100-1 \
--commission-rate=0.05 \
--commission-max-rate=0.20 \
--commission-max-change-rate=0.01 \
--min-self-delegation=1 \
--from=mywallet \
--gas-adjustment=1.2 \
--gas=300000 \
--node tcp://localhost:16707 \
--home ${HOME}/.blockd \
-y
```

#### Edit Validator Identities Informations

```bash
MONIKER="NAME_OF_YOUR_VALIDATOR"
PROFILE="PGP_KEY_OF_KEYBASE"
DETAILS="Describes Your Validator"
WEBSITE="https://yourwebsite.com"

blockd tx staking edit-validator \
--moniker="${MONIKER}" \
--identity="${PROFILE}" \
--details="${DETAILS}" \
--website="${WEBSITE}" \
--chain-id=blockx_100-1 \
--commission-rate=0.05 \
--from=mywallet \
--gas-adjustment=1.2 \
--gas=300000 \
--node tcp://localhost:16707 \
--home ${HOME}/.blockd \
-y
```

#### Get Validator Info

```
blockd status 2>&1 | jq .ValidatorInfo
```

#### Get Syncing Block

```
blockd status 2>&1 | jq .SyncInfo
```

#### Get Peer Own Node

```bash
echo $(blockd tendermint show-node-id)'@'$(curl -s ifconfig.me)':'$(cat $HOME/.blockd/config/config.toml | sed -n '/Address to listen for incoming connection/{n;p;}' | sed 's/.*://; s/".*//')
```

#### Get Peer Node

```
curl -sS http://localhost:16707/net_info | jq -r '.result.peers[] | "\(.node_info.id)@\(.remote_ip):\(.node_info.listen_addr)"' | awk -F ':' '{print $1":"$(NF)}'
```

#### Unjail Validator

```bash
blockd tx slashing unjail \
--from mywallet \
--chain-id blockx_100-1 \
 --home ${HOME}/.blockd \
 --node  tcp://localhost:16707 \
 --gas 300000 \
 --gas-adjustment 1.2 \
 -y
```

#### Jail Reason

```bash
blockd query slashing signing-info $(blockd tendermint show-validator) \
 --node  tcp://localhost:16707 \
 --home ${HOME}/.blockd
```

#### List All Active Validator

```bash
blockd q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_BONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### List All Inactive Validator

```bash
blockd q staking validators -oj --limit=3000 | jq '.validators[] | select(.status=="BOND_STATUS_UNBONDED")' | jq -r '(.tokens|tonumber/pow(10; 6)|floor|tostring) + " \t " + .description.moniker' | sort -gr | nl
```

#### Show Validator Details

```
blockd \
query staking validator \
$(blockd keys show \ 
                $(blockd keys list --home ${HOME}/.blockd --output json| jq -r ".[] .address" | tail -n1) \
--bech val -a) \
--chain-id blockx_100-1 \
--node tcp://localhost:16707
```

### Token Management

#### Withdraw All Reward From Validator

```bash
blockd tx distribution withdraw-all-rewards \
--from mywallet \
--chain-id blockx_100-1 \
--node tcp://localhost:16707 \
--home ${HOME}/.blockd \
--gas-adjustment 1.2 \
--gas 300000 \
-y
```

#### Withdraw Commision Reward From Validator

```bash
blockd tx distribution withdraw-rewards $(blockd keys show mywallet --bech val -a) \
--commission \
--from mywallet \
--gas-adjustment 1.2 \
--gas 300000 \
--chain-id blockx_100-1 \
--node tcp://localhost:16707 \
--home ${HOME}/.blockd \
-y 
```

#### Delegate My Token to Own Validator

```bash
blockd tx staking delegate $(blockd keys show wallet --bech val -a) 100000000000000000000000abcx \
--from mywallet \
--gas-adjustment 1.2 \
--home ${HOME}/.blockd \
--node tcp://localhost:16707 \
--chain-id blockx_100-1 \
--gas 300000 \
-y
```

#### Delegate Your Token To Our Validator

```bash
blockd tx staking delegate blockxvaloper16am9xxy7q4yw5l9zx76zqm2p3ne8e6zns8xq3t 100000000000000000000000abcx \ 
--from mywallet \
--gas-adjustment 1.2 \ 
--gas 300000 \
--home ${HOME}/.blockd \
--node tcp://localhost:16707 \
--chain-id blockx_100-1 \
-y
```

#### Redelegate Tokens to Another Validator

```bash
blockd tx staking redelegate $(blockd keys show wallet --bech val -a) <TO_VALOPER_ADDRESS> 100000000000000000000000abcx \
--from mywallet 
--gas-adjustment 1.2 \
--gas 300000 \
--home ${HOME}/.blockd \
--node tcp://localhost:16707 \
--chain-id blockx_100-1 \
-y 
```

#### Unbound or Unstake Your Tokens

```bash
blockd tx staking unbond $(blockd keys show wallet --bech val -a) 100000000000000000000000abcx \
--from mywallet \
--gas-adjustment 1.2 \
--gas 300000 \
--home ${HOME}/.blockd \
--node tcp://localhost:16707 \
--chain-id blockx_100-1 \
-y 
```

Send tokens to the wallet

```
blockd tx bank send wallet <TO_WALLET_ADDRESS> 100000000000000000000000abcx \
--from mywallet \
--gas-adjustment 1.2 \
--gas 300000 \
--home ${HOME}/.blockd \
--node tcp://localhost:16707 \
--chain-id blockx_100-1 \
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
blockd query gov proposals
```

#### How to Vote

```bash
### vote yes
blockd tx gov vote 1 yes \
--from mywallet \
--gas-adjustment 1.2 \
--gas 300000 \
--home ${HOME}/.blockd \
--node tcp://localhost:16707 \
--chain-id blockx_100-1 \
-y 

### vote no
blockd tx gov vote 1 no \
--from mywallet \
--gas-adjustment 1.2 \
--gas 300000 \
--home ${HOME}/.blockd \
--node tcp://localhost:16707 \
--chain-id blockx_100-1 \
-y 

### vote abstain
blockd tx gov vote 1 abstain \
--from mywallet \
--gas-adjustment 1.2 \
--gas 300000 \
--home ${HOME}/.blockd \
--node tcp://localhost:16707 \
--chain-id blockx_100-1 \
-y 

### vote No With Veto
blockd tx gov vote 1 nowithveto \
--from mywallet \
--gas-adjustment 1.2 \
--gas 300000 \
--home ${HOME}/.blockd \
--node tcp://localhost:16707 \
--chain-id blockx_100-1 \
-y 
```
