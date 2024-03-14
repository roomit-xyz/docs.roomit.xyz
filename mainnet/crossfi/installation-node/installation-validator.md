---
description: Install Node Producer
---

# Installation Validator

**Chain ID**: crossfi-evm-testnet-1 | **Latest Binary Version**: 0.3.0-prebuild3

Update and install packages for compiling

```
sudo apt update
sudo apt install curl git jq lz4 build-essential zsh -y
```

#### User Management

{% hint style="info" %}
**Running in user** (Assume) : _salinem_ We never used this username in our production !
{% endhint %}

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

#### FHS of Crossfi

Create FHS for application

```bash
echo "Install FHS"
mkdir -p ${HOME}/tmp
mkdir -p ${HOME}/lib
mkdir -p ${HOME}/bin
mkdir -p ${HOME}/conf
mkdir -p ${HOME}/systemd
```

### Install Golang

```bash
# Install Go 1.21.9
wget https://go.dev/dl/go1.21.9.linux-amd64.tar.gz
tar xf go1.21.9.linux-amd64.tar.gz
sudo mv go /usr/local/go

# Set Go path to $PATH variable
echo "export PATH=$PATH:/usr/local/go/bin:~/go/bin" >> $HOME/.zshrc
source ~/.zshrc
```

#### Install Crossfi

```bash
cd ~
wget -c https://github.com/crossfichain/crossfi-node/releases/download/v0.3.0-prebuild3/crossfi-node_0.3.0-prebuild3_linux_amd64.tar.gz
tar xvf crossfi-node_0.3.0-prebuild3_linux_amd64.tar.gz -C ${HOME}/bin

```

#### Environment

{% hint style="info" %}
In this case, RoomIT used zsh, if you used bash just direct to ${HOME}/.bashrc or ${HOME}/.profile
{% endhint %}

```
echo 'export PATH="${PATH}:${HOME}/bin"' >> ${HOME}/.zshrc
source ~/.zshrc
```

Set Variable

```bash
# Chain ID of Crossfi Mainnet
export CHAIN_ID=crossfi-evm-testnet-1
# Wallet name to be used as validator's account, please change this into your name (no whitespace).
export WALLET_NAME=<YOUR_WALLET_NAME>
# Name of your validator node, please change this into your name.
export MONIKER=<YOUR_MONIKER>
# URL of genesis file for Crossfi Mainnet
export GENESIS_FILE_URL= 

```

#### Intialize Node

```bash
crossfid init --chain-id $CHAIN_ID "$MONIKER"
```

**Replace genesis file with our genesis file**

```bash
wget $GENESIS_FILE_URL -O $HOME/.mineplex-chain/config/genesis.json
```

**Download data Crossfi / oracle scripts files, and store in $HOME/.mineplex-chain/files**

```bash
wget -qO- $BIN_FILES_URL | tar xvz -C $HOME/.mineplex-chain/
```

**Create new account**

```bash
crossfid keys add $WALLET_NAME
```

Validate Genesis

```
crossfid validate-genesis
```

Edit config, You can refer to [edit-configuration-node.md](edit-configuration-node.md "mention")

Apply State Sync, refer to [state-sync.md](state-sync.md "mention")
