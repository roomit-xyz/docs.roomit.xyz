---
description: Install Node Producer
---

# Installation Validator

**Chain ID**: gitopia | **Latest Binary Version**: v3.3.0

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

#### FHS of Gitopia

Create FHS for application

```bash
echo "Install FHS"
mkdir -p ${HOME}/tmp
mkdir -p ${HOME}/lib
mkdir -p ${HOME}/bin
mkdir -p ${HOME}/conf
mkdir -p ${HOME}/systemd
```

#### Install Gitopia

```bash
cd ~
apt install jq -y
apt install unzip -y
wget https://server.gitopia.com/releases/Gitopia/gitopia/v3.3.0/gitopiad_3.3.0_linux_amd64.tar.gz
tar xvf gitopiad_3.3.0_linux_amd64 -C ${HOME}/bin/
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
gitopiad init <your_custom_moniker> --chain-id gitopia
```

#### Add Genesis

<pre class="language-bash"><code class="lang-bash"><strong>wget https://github.com/gitopia/mainnet/raw/master/genesis.tar.gz &#x26;&#x26; tar xvf genesis.tar.gz
</strong>mv genesis.json ~/.gitopia/config/
</code></pre>

Validate Genesis

```
gitopiad validate-genesis
```

Edit config, You can refer to [edit-configuration-node.md](edit-configuration-node.md "mention")

Apply State Sync, refer to [state-sync.md](state-sync.md "mention")
