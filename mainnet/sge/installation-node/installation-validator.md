---
description: Install Node Producer
---

# Installation Validator

**Chain ID**: sgenet-1 | **Latest Binary Version**: v1.1.2

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

#### FHS of SGE

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
# Install Go 1.19.9
wget https://go.dev/dl/go1.19.9.linux-amd64.tar.gz
tar xf go1.19.9.linux-amd64.tar.gz
sudo mv go /usr/local/go

# Set Go path to $PATH variable
echo "export PATH=$PATH:/usr/local/go/bin:~/go/bin" >> $HOME/.zshrc
source ~/.zshrc
```

#### Install SGE

```bash
cd ~
# Clone SGE Protocol  version v1.1.1
git clone  https://github.com/sge-network/sge
cd chain
git fetch && git checkout v1.1.1

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
# Chain ID of SGE Mainnet
export CHAIN_ID=sgenet-1
# Wallet name to be used as validator's account, please change this into your name (no whitespace).
export WALLET_NAME=<YOUR_WALLET_NAME>
# Name of your validator node, please change this into your name.
export MONIKER=<YOUR_MONIKER>
# URL of genesis file for SGE Mainnet
export GENESIS_FILE_URL= 

```

#### Intialize Node

```bash
sged init --chain-id $CHAIN_ID "$MONIKER"
```

**Replace genesis file with our genesis file**

```bash
wget $GENESIS_FILE_URL -O $HOME/.sge/config/genesis.json
```

**Download data SGE / oracle scripts files, and store in $HOME/.sge/files**

```bash
wget -qO- $BIN_FILES_URL | tar xvz -C $HOME/.sge/
```

**Create new account**

```bash
sged keys add $WALLET_NAME
```

Validate Genesis

```
sged validate-genesis
```

Edit config, You can refer to [edit-configuration-node.md](edit-configuration-node.md "mention")

Apply State Sync, refer to [state-sync.md](state-sync.md "mention")
