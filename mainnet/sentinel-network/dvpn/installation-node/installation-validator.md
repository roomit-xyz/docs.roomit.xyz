---
description: Install Node Producer
---

# Installation Validator

**Chain ID**: sentinelhub-2| **Latest Binary Version**: v0.11.3

Update and install packages for compiling

```
sudo apt update
sudo apt install curl git jq lz4 build-essential zsh -y
```

#### User Management

{% hint style="info" %}
**Running in user** (Assume) : _salinem_ We never used this username in our production !
{% endhint %}

Create User, Please Refer to [user-and-group-management.md](../../../../security/user-and-group-management.md "mention")

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

#### FHS of dVPN

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

#### Install dVPN

```bash
cd ~
git clone https://github.com/sentinel-official/hub.git "${HOME}/sentinelhub"
cd "${HOME}/sentinelhub"
git checkout v0.11.3 # always make sure it's the latest version


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
# Chain ID of dVPN Mainnet
export CHAIN_ID=sentinelhub-2
# Wallet name to be used as validator's account, please change this into your name (no whitespace).
export WALLET_NAME=<YOUR_WALLET_NAME>
# Name of your validator node, please change this into your name.
export MONIKER=<YOUR_MONIKER>
# URL of genesis file for dVPN Mainnet
export GENESIS_FILE_URL= https://github.com/sentinel-official/networks/raw/main/sentinelhub-2/genesis.zip
# Data dVPN binary scripts files
export BIN_FILES_URL = 
```

#### Intialize Node

```bash
sentinelhub init --chain-id $CHAIN_ID "$MONIKER"
```

**Replace genesis file with our genesis file**

```bash
wget $GENESIS_FILE_URL -O $HOME/.sentinelhub/config/genesis.json
```

**Create new account**

```bash
sentinelhub keys add $WALLET_NAME
```

Validate Genesis

```
sentinelhub validate-genesis
```

Edit config, You can refer to [edit-configuration-node.md](edit-configuration-node.md "mention")

Apply State Sync, refer to [state-sync.md](../../../dvpn/installation-node/state-sync.md "mention")
