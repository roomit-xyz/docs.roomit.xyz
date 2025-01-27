---
description: Install Node Producer/Validator Atomone
---

# Installation Validator

**Chain ID**: atomone-1 | **Latest Binary Version**: vv1.0.0

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
echo "export CHAIN_ID=atomone-1" >> $HOME/.zshrc
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
echo "export CHAIN_ID=atomone-1" >> $HOME/.bashrc
echo "export WALLET_NAME=mywallet" >> $HOME/.bashrc
echo "export MONIKER=MYNODE"  >> $HOME/.bashrc
source ~/.bashrc
```


#### Install Atomone


```bash
cd ${HOME}/source
# Clone SGE Protocol  version v1.1.1
git clone  https://github.com/atomone-hub/atomone atomone
cd ${HOME}/source/atomone
git fetch && git checkout v{'changed': True, 'stdout': 'v1.0.0', 'stderr': '', 'rc': 0, 'cmd': '/app/mainnet/atomone/bin/atomoned version', 'start': '2025-01-26 07:05:06.646748', 'end': '2025-01-26 07:05:06.719807', 'delta': '0:00:00.073059', 'msg': '', 'stdout_lines': ['v1.0.0'], 'stderr_lines': [], 'failed': False}
make build
cp build/atomoned ${HOME}/bin/
```

#### Intialize Node

```bash
atomoned init --chain-id $CHAIN_ID "$MONIKER"
```

**Replace genesis file with our genesis file**

```bash
wget https://share102.utsa.tech/atomone/genesis.json -O $HOME/.atomone/config/genesis.json
```

<!-- **Download data Atomone / oracle scripts files, and store in $HOME/.atomone/files**

```bash
wget -qO- $BIN_FILES_URL | tar xvz -C $HOME/.atomone/
``` -->

**Create new account**

```bash
atomoned keys add $WALLET_NAME
```

Validate Genesis

```
atomoned validate-genesis
```

Edit config, You can refer to [edit-configuration-node.md](edit-configuration-node.md "mention")

Apply State Sync, refer to [state-sync.md](../infrastructures/statesync.md "mention")
