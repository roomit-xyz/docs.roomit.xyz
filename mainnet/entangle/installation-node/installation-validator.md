---
description: Install Node Producer
---

# Installation Validator

**Chain ID**: cataclysm-1 | **Latest Binary Version**: v1.2.0

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

#### FHS of PlanQ

Create FHS for application

```bash
echo "Install FHS"
mkdir -p ${HOME}/tmp
mkdir -p ${HOME}/lib
mkdir -p ${HOME}/bin
mkdir -p ${HOME}/conf
mkdir -p ${HOME}/systemd
```

#### Install PlanQ

```bash
cd ~
apt install jq -y
apt install unzip -y
wget https://github.com/NibiruChain/nibiru/releases/download/v1.2.0/nibid_1.2.0_linux_amd64.tar.gz
tar xvf nibid_1.2.0_linux_amd64.tar.gz
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
nibid init <your_custom_moniker> --chain-id cataclysm-1
```

#### Add Genesis

```bash
wget https://roomit.xyz/genesis/mainnet/nibiru/genesis.json
mv genesis.json ~/.nibid/config/
```

Validate Genesis

```
nibid validate-genesis
```

Edit config, You can refer to [edit-configuration-node.md](edit-configuration-node.md "mention")

Apply State Sync, refer to [state-sync.md](state-sync.md "mention")
