---
description: Install Node Producer
---

# Installation Validator

**Chain ID**: laozi-mainnet | **Latest Binary Version**: v2.5.4

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

#### FHS of Band

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

#### Install Band

```bash
cd ~
# Clone BandChain Laozi version v2.5.2
git clone https://github.com/bandprotocol/chain
cd chain
git fetch && git checkout v2.5.2

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
# Chain ID of Laozi Mainnet
export CHAIN_ID=laozi-mainnet
# Wallet name to be used as validator's account, please change this into your name (no whitespace).
export WALLET_NAME=<YOUR_WALLET_NAME>
# Name of your validator node, please change this into your name.
export MONIKER=<YOUR_MONIKER>
# URL of genesis file for Laozi Mainnet
export GENESIS_FILE_URL=https://raw.githubusercontent.com/bandprotocol/launch/master/laozi-mainnet/genesis.json
# Data sources/oracle scripts files
export BIN_FILES_URL=https://raw.githubusercontent.com/bandprotocol/launch/master/laozi-mainnet/files.tar.gz
```

#### Intialize Node

```bash
bandd init --chain-id $CHAIN_ID "$MONIKER"
```

**Replace genesis file with our genesis file**

```bash
wget $GENESIS_FILE_URL -O $HOME/.band/config/genesis.json
```

**Download data sources / oracle scripts files, and store in $HOME/.band/files**

```bash
wget -qO- $BIN_FILES_URL | tar xvz -C $HOME/.band/
```

**Create new account**

```bash
bandd keys add $WALLET_NAME
```

Validate Genesis

```
bandd validate-genesis
```

Edit config, You can refer to [edit-configuration-node.md](edit-configuration-node.md "mention")

Apply State Sync, refer to [state-sync.md](../infrastructures/statesync.md "mention")
