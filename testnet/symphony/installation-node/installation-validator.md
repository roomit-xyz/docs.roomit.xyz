---
description: Install Node Producer/Validator Symphony
---

# Installation Validator

**Chain ID**: symphony-testnet-2 | **Latest Binary Version**: v1.1.1

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
# Install Go 1.21.3
wget https://go.dev/dl/go1.21.3.linux-amd64.tar.gz
tar xf go1.21.3.linux-amd64.tar.gz
mv go ${HOME}/bin
```


### Environment and Variable
If Using ZSH
```
# Set Go path to $PATH variable
echo "export PATH=$PATH:/usr/local/go/bin:~/go/bin" >> $HOME/.zshrc
echo "export GOPATH="${HOME}/lib" >> $HOME/.zshrc
echo "export GOMAXPROCS=2" >> $HOME/.zshrc
echo "export CHAIN_ID=symphony-testnet-2" >> $HOME/.zshrc
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
echo "export CHAIN_ID=symphony-testnet-2" >> $HOME/.bashrc
echo "export WALLET_NAME=mywallet" >> $HOME/.bashrc
echo "export MONIKER=MYNODE"  >> $HOME/.bashrc
source ~/.bashrc
```


#### Install Symphony


```bash
cd ${HOME}/source
# Clone SGE Protocol  version v1.1.1
git clone  https://github.com/Orchestra-Labs/symphony symphony
cd ${HOME}/source/symphony
git fetch && git checkout v{'changed': True, 'stdout': '', 'stderr': '0.2.1', 'rc': 0, 'cmd': '/app/testnet/symphony/bin/symphonyd version', 'start': '2024-07-30 07:03:16.540867', 'end': '2024-07-30 07:03:16.653898', 'delta': '0:00:00.113031', 'msg': '', 'stdout_lines': [], 'stderr_lines': ['0.2.1'], 'ansible_facts': {'discovered_interpreter_python': '/usr/bin/python3'}, 'failed': False}
make build
cp build/symphonyd ${HOME}/bin/
```

#### Intialize Node

```bash
symphonyd init --chain-id $CHAIN_ID "$MONIKER"
```

**Replace genesis file with our genesis file**

```bash
wget https://files.nodeshub.online/testnet/symphony/genesis.json -O $HOME/.symphonyd/config/genesis.json
```

<!-- **Download data Symphony / oracle scripts files, and store in $HOME/.symphonyd/files**

```bash
wget -qO- $BIN_FILES_URL | tar xvz -C $HOME/.symphonyd/
``` -->

**Create new account**

```bash
symphonyd keys add $WALLET_NAME
```

Validate Genesis

```
symphonyd validate-genesis
```

Edit config, You can refer to [edit-configuration-node.md](edit-configuration-node.md "mention")

Apply State Sync, refer to [state-sync.md](../infrastructures/statesync.md "mention")
