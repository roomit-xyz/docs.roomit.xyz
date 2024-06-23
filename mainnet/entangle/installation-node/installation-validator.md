---
description: Install Node Producer/Validator Entangle
---

# Installation Validator

**Chain ID**: entangle_33033-1 | **Latest Binary Version**: v1.0.1

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
# Install Go 1.21.7
wget https://go.dev/dl/go1.21.7.linux-amd64.tar.gz
tar xf go1.21.7.linux-amd64.tar.gz
mv go ${HOME}/bin
```


### Environment and Variable
If Using ZSH
```
# Set Go path to $PATH variable
echo "export PATH=$PATH:/usr/local/go/bin:~/go/bin" >> $HOME/.zshrc
echo "export GOPATH="${HOME}/lib" >> $HOME/.zshrc
echo "export GOMAXPROCS=2" >> $HOME/.zshrc
echo "export CHAIN_ID=entangle_33033-1" >> $HOME/.zshrc
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
echo "export CHAIN_ID=entangle_33033-1" >> $HOME/.bashrc
echo "export WALLET_NAME=mywallet" >> $HOME/.bashrc
echo "export MONIKER=MYNODE"  >> $HOME/.bashrc
source ~/.bashrc
```


#### Install Entangle


```bash
cd ${HOME}/source
# Clone SGE Protocol  version v1.1.1
git clone  https://github.com/Entangle-Protocol/entangle-blockchain entangle
cd ${HOME}/source/entangle
git fetch && git checkout v{'changed': True, 'stdout': '1.0.1', 'stderr': '', 'rc': 0, 'cmd': '/app/mainnet/entangle/bin/entangled version', 'start': '2024-06-11 06:40:56.231256', 'end': '2024-06-11 06:40:56.332192', 'delta': '0:00:00.100936', 'msg': '', 'stdout_lines': ['1.0.1'], 'stderr_lines': [], 'ansible_facts': {'discovered_interpreter_python': '/usr/bin/python3'}, 'failed': False}
make build
cp build/entangled ${HOME}/bin/
```

#### Intialize Node

```bash
entangled init --chain-id $CHAIN_ID "$MONIKER"
```

**Replace genesis file with our genesis file**

```bash
wget https://raw.githubusercontent.com/Entangle-Protocol/entangle-blockchain/main/config/genesis.json -O $HOME/.entangled/config/genesis.json
```

<!-- **Download data Entangle / oracle scripts files, and store in $HOME/.entangled/files**

```bash
wget -qO- $BIN_FILES_URL | tar xvz -C $HOME/.entangled/
``` -->

**Create new account**

```bash
entangled keys add $WALLET_NAME
```

Validate Genesis

```
entangled validate-genesis
```

Edit config, You can refer to [edit-configuration-node.md](edit-configuration-node.md "mention")

Apply State Sync, refer to [state-sync.md](../infrastructures/statesync.md "mention")
