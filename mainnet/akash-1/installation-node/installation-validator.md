---
description: Install Node Producer
---

# Installation Validator

**Chain ID**: planq\_7070-2 | **Latest Binary Version**: v6.0.2

Update and install packages for compiling

```
sudo apt update
sudo apt install curl git jq lz4 build-essential zsh -y
```

#### User Management

Create User, Please Refer to [user-and-group-management.md](../../../security/user-and-group-management.md "mention")

Login as User salinem

```bash
su - salinem
```

or

```bash
sudo su - salinem
```

and make sure we are in directory

```bash
pwd

/mainnet/salinem
```

{% hint style="info" %}
**Running in user** : _salinem_
{% endhint %}

#### FHS of Comdex

Create FHS for application

```bash
echo "Install FHS"
mkdir -p ${HOME}/tmp
mkdir -p ${HOME}/lib
mkdir -p ${HOME}/bin
mkdir -p ${HOME}/conf
mkdir -p ${HOME}/systemd
```

#### Install PlanQ

```bash
cd ~
apt install jq -y
apt install unzip -y
curl https://raw.githubusercontent.com/ovrclk/akash/master/godownloader.sh | sh -s -- "$AKASH_VERSION"
```

#### Environment

{% hint style="info" %}
In this case, RoomIT used zsh, if you used  bash just direct to ${HOME}/.bashrc or ${HOME}/.profile&#x20;
{% endhint %}

```
echo 'export PATH="${PATH}:${HOME}/bin"' >> ${HOME}/.zshrc
```

#### Intialize Node

```bash
AKASH_NET="https://raw.githubusercontent.com/ovrclk/net/master/mainnet"
export AKASH_CHAIN_ID="$(curl -s "$AKASH_NET/chain-id.txt")"
```

make sure you have done get value of chain

```bash
echo $AKASH_CHAIN_ID
```

Create init file node

```
akash init --chain-id "$AKASH_CHAIN_ID" "YOUE_NAME_VALIDATOR"
```

#### Add Genesis

```bash
wget -O $HOME/.akash/config/genesis.json https://github.com/ovrclk/net/raw/master/mainnet/genesis.json 
```

#### Add AddressBook

```bash
wget -O addrbook.json https://snapshots.polkachu.com/addrbook/akash/addrbook.json --inet4-only
mv addrbook.json ~/.akash/config
```

Edit config, You can refer to [edit-configuration-node.md](../../comdex/installation-node/edit-configuration-node.md "mention")

Apply State Sync, refer to [state-sync.md](../../akash/installation-node/state-sync.md "mention")
