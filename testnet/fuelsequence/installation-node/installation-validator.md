---
description: Install Node Producer/Validator Fuelsequence
---

# Installation Validator

**Chain ID**: seq-testnet-2 | **Latest Binary Version**: vseq-testnet-2-improved-sidecar

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
# Install Go 1.21.13
wget https://go.dev/dl/go1.21.13.linux-amd64.tar.gz
tar xf go1.21.13.linux-amd64.tar.gz
mv go ${HOME}/bin
```


### Environment and Variable
If Using ZSH
```
# Set Go path to $PATH variable
echo "export PATH=$PATH:/usr/local/go/bin:~/go/bin" >> $HOME/.zshrc
echo "export GOPATH="${HOME}/lib" >> $HOME/.zshrc
echo "export GOMAXPROCS=2" >> $HOME/.zshrc
echo "export CHAIN_ID=seq-testnet-2" >> $HOME/.zshrc
echo "export WALLET_NAME=mywallet" >> $HOME/.zshrc
echo "export MONIKER=MYNODE"  >> $HOME/.zshrc
source ~/.zshrc
```


If Using BASH
```
# Set Go path to $PATH variable
echo "export PATH=$PATH:/usr/local/go/bin:~/go/bin" >> $HOME/.bashrc
echo "export GOPATH="${HOME}/lib" >> >> $HOME/.bahsrc
echo "export GOMAXPROCS=2" >> $HOME/.bashrc
echo "export CHAIN_ID=seq-testnet-2" >> $HOME/.bashrc
echo "export WALLET_NAME=mywallet" >> $HOME/.bashrc
echo "export MONIKER=MYNODE"  >> $HOME/.bashrc
source ~/.bashrc
```


#### Install Fuelsequence

```bash
wget -c https://github.com/FuelLabs/fuel-sequencer-deployments/releases/download/seq-testnet-2-improved-sidecar/fuelsequencerd-seq-testnet-2-improved-sidecar-linux-amd64 -O ${HOME}/bin/fuelsequencerd
```


#### Intialize Node

```bash
fuelsequencerd init --chain-id $CHAIN_ID "$MONIKER"
```

**Replace genesis file with our genesis file**

```bash
wget https://raw.githubusercontent.com/FuelLabs/fuel-sequencer-deployments/refs/heads/main/seq-testnet-2/genesis.json -O $HOME/.fuelsequencer/config/genesis.json
```

<!-- **Download data Fuelsequence / oracle scripts files, and store in $HOME/.fuelsequencer/files**

```bash
wget -qO- $BIN_FILES_URL | tar xvz -C $HOME/.fuelsequencer/
``` -->

**Create new account**

```bash
fuelsequencerd keys add $WALLET_NAME
```

Validate Genesis

```
fuelsequencerd validate-genesis
```

Edit config, You can refer to [edit-configuration-node.md](edit-configuration-node.md "mention")

Apply State Sync, refer to [state-sync.md](../infrastructures/statesync.md "mention")
