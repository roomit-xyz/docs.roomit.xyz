---
description: Install Node Producer
---

# Installation Validator

**Chain ID**: blockx_100-1 | **Latest Binary Version**: v1.0.1

Update and install packages for compiling

```
sudo apt update
sudo apt install curl git jq lz4 build-essential zsh -y
```

#### User Management

{% hint style="info" %}
**Running in user** (Assume) : _salinem_
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

#### FHS of blockx

Create FHS for application

```bash
echo "Install FHS"
mkdir -p ${HOME}/tmp
mkdir -p ${HOME}/lib
mkdir -p ${HOME}/bin
mkdir -p ${HOME}/conf
mkdir -p ${HOME}/systemd
```

#### Install blockx

```bash
cd ~
apt install jq -y
apt install unzip -y
git clone https://github.com/BlockXLabs/BlockX-Genesis-Mainnet1.git blockx
cd blockx
export GOPATH="${HOME}/lib"
make install 
cp ~/lib/go/bin/blockxd ~/bin
```

#### Environment

{% hint style="info" %}
In this case, RoomIT used zsh, if you used bash just direct to ${HOME}/.bashrc or ${HOME}/.profile
{% endhint %}

```
echo 'export PATH="${PATH}:${HOME}/bin"' >> ${HOME}/.zshrc
source ~/.zshrc
```

#### Intialize Node

```bash
blockxd init <your_custom_moniker> --chain-id blockx_7070-2
```

#### Add Genesis

```bash
wget https://raw.githubusercontent.com/BlockXLabs/networks/master/chains/blockx_100-1/genesis.json
mv genesis.json ~/.blockxd/config/
```

Validate Genesis

```
blockxd validate-genesis
```

Edit config, You can refer to [edit-configuration-node.md](edit-configuration-node.md "mention")

Apply State Sync, refer to [state-sync.md](state-sync.md "mention")
