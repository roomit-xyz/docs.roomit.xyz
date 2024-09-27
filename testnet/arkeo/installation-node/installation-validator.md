---
description: Install Node Producer/Validator Arkeo
---

# Installation Validator

**Chain ID**: arkeo | **Latest Binary Version**: vv1.0.0-prerelease

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
# Install Go 1.22.3
wget https://go.dev/dl/go1.22.3.linux-amd64.tar.gz
tar xf go1.22.3.linux-amd64.tar.gz
mv go ${HOME}/bin
```


### Environment and Variable
If Using ZSH
```
# Set Go path to $PATH variable
echo "export PATH=$PATH:/usr/local/go/bin:~/go/bin" >> $HOME/.zshrc
echo "export GOPATH="${HOME}/lib" >> $HOME/.zshrc
echo "export GOMAXPROCS=2" >> $HOME/.zshrc
echo "export CHAIN_ID=arkeo" >> $HOME/.zshrc
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
echo "export CHAIN_ID=arkeo" >> $HOME/.bashrc
echo "export WALLET_NAME=mywallet" >> $HOME/.bashrc
echo "export MONIKER=MYNODE"  >> $HOME/.bashrc
source ~/.bashrc
```


#### Install Arkeo


```bash
cd ${HOME}/source
# Clone SGE Protocol  version v1.1.1
git clone  https://github.com/arkeonetwork/arkeo arkeo
cd ${HOME}/source/arkeo
git fetch && git checkout v{'changed': True, 'stdout': 'v1.0.0-prerelease', 'stderr': '', 'rc': 0, 'cmd': '/app/testnet/arkeo/bin/arkeod version', 'start': '2024-09-05 11:30:33.541294', 'end': '2024-09-05 11:30:33.664328', 'delta': '0:00:00.123034', 'msg': '', 'stdout_lines': ['v1.0.0-prerelease'], 'stderr_lines': [], 'failed': False}
make build
cp build/arkeod ${HOME}/bin/
```

#### Intialize Node

```bash
arkeod init --chain-id $CHAIN_ID "$MONIKER"
```

**Replace genesis file with our genesis file**

```bash
wget https://ss-t.arkeo.nodestake.org/genesis.json -O $HOME/.arkeo/config/genesis.json
```

<!-- **Download data Arkeo / oracle scripts files, and store in $HOME/.arkeo/files**

```bash
wget -qO- $BIN_FILES_URL | tar xvz -C $HOME/.arkeo/
``` -->

**Create new account**

```bash
arkeod keys add $WALLET_NAME
```

Validate Genesis

```
arkeod validate-genesis
```

Edit config, You can refer to [edit-configuration-node.md](edit-configuration-node.md "mention")

Apply State Sync, refer to [state-sync.md](../infrastructures/statesync.md "mention")
