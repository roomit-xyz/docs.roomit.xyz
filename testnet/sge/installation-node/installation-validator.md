---
description: Install Node Producer/Validator Sge
---

# Installation Validator

**Chain ID**: sge-network-4 | **Latest Binary Version**: v1.1.1

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
echo "export CHAIN_ID=sge-network-4" >> $HOME/.zshrc
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
echo "export CHAIN_ID=sge-network-4" >> $HOME/.bashrc
echo "export WALLET_NAME=mywallet" >> $HOME/.bashrc
echo "export MONIKER=MYNODE"  >> $HOME/.bashrc
source ~/.bashrc
```


#### Install Sge


```bash
cd ${HOME}/source
# Clone SGE Protocol  version v1.1.1
git clone  https://github.com/sge-network/sge sge
cd ${HOME}/source/sge
git fetch && git checkout v{'changed': True, 'stdout': '', 'stderr': 'v1.7.0', 'rc': 0, 'cmd': '/app/testnet/sge/bin/sged version', 'start': '2024-06-12 04:16:07.489425', 'end': '2024-06-12 04:16:07.550478', 'delta': '0:00:00.061053', 'msg': '', 'stdout_lines': [], 'stderr_lines': ['v1.7.0'], 'ansible_facts': {'discovered_interpreter_python': '/usr/bin/python3'}, 'failed': False}
make build
cp build/sged ${HOME}/bin/
```

#### Intialize Node

```bash
sged init --chain-id $CHAIN_ID "$MONIKER"
```

**Replace genesis file with our genesis file**

```bash
wget https://snapshots.polkachu.com/testnet-genesis/sge/genesis.json -O $HOME/.sge/config/genesis.json
```

<!-- **Download data Sge / oracle scripts files, and store in $HOME/.sge/files**

```bash
wget -qO- $BIN_FILES_URL | tar xvz -C $HOME/.sge/
``` -->

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
