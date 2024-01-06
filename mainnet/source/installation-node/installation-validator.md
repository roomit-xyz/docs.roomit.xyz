---
description: Install Node Producer
---

# Installation Validator

**Chain ID**: source-1| **Latest Binary Version**: v3.0.0

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

#### FHS of Source

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
# Install Go 1.19.1
wget https://go.dev/dl/go1.19.1.linux-amd64.tar.gz
tar xf go1.19.1.linux-amd64.tar.gz
sudo mv go /usr/local/go

# Set Go path to $PATH variable
echo "export PATH=$PATH:/usr/local/go/bin:~/go/bin" >> $HOME/.zshrc
source ~/.zshrc
```

#### Install Source

```bash
cd ~
# Clone Source Protocol  version v3.0.0
git clone  https://github.com/Source-Protocol-Cosmos/source.git
cd chain
git fetch && git checkout v3.0.0

# Install binaries to $GOPATH/bin
export GOPATH="${HOME}/lib"
make install

cp ${HOME}/lib/bin/* ~/bin/
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
# Chain ID of Source Mainnet
export CHAIN_ID=source-1
# Wallet name to be used as validator's account, please change this into your name (no whitespace).
export WALLET_NAME=<YOUR_WALLET_NAME>
# Name of your validator node, please change this into your name.
export MONIKER=<YOUR_MONIKER>
# URL of genesis file for Source Mainnet
export GENESIS_FILE_URL= https://raw.githubusercontent.com/Source-Protocol-Cosmos/mainnet/master/source-1/genesis.json
# Data sources/oracle scripts files
export BIN_FILES_URL=
```

#### Intialize Node

```bash
sourced init --chain-id $CHAIN_ID "$MONIKER"
```

**Replace genesis file with our genesis file**

```bash
wget $GENESIS_FILE_URL -O $HOME/.source/config/genesis.json
```

**Download data sources / oracle scripts files, and store in $HOME/.source/files**

```bash
wget -qO- $BIN_FILES_URL | tar xvz -C $HOME/.source/
```

**Create new account**

```bash
sourced keys add $WALLET_NAME
```

Validate Genesis

```
sourced validate-genesis
```

Edit config, You can refer to [edit-configuration-node.md](edit-configuration-node.md "mention")

Apply State Sync, refer to [state-sync.md](state-sync.md "mention")
