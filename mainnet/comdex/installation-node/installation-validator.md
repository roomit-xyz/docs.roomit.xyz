---
description: Install Node Producer
---

# Installation Validator

**Chain ID**: gravity-bridge-3 | **Latest Binary Version**: v1.7.2

Update and install packages for compiling

```
sudo apt update
sudo apt install curl git jq lz4 build-essential zsh -y
```

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

Create FHS for application

```bash
echo "Install FHS"
mkdir -p ${HOME}/tmp
mkdir -p ${HOME}/lib
mkdir -p ${HOME}/bin
```

Install Golang

<pre class="language-bash"><code class="lang-bash">GOLANG_VERSION="1.18.2"
<strong>wget "https://golang.org/dl/go${GOLANG_VERSION}.linux-amd64.tar.gz"
</strong>tar xvf go${GOLANG_VERSION}.linux-amd64.tar.gz 
mv go/ ${HOME_VALIDATOR}/.go
rm -f ${HOME_VALIDATOR}/tmp/go${GOLANG_VERSION}.linux-amd64.tar.gz
</code></pre>

Set Variable Golang in shell zshrc

```bash
HOME_VALIDATOR=`pwd`
tee -a $HOME/.zshrc > /dev/null << EOF
GOBIN="$HOME_VALIDATOR/.go/bin"
GOPATH="\$HOME_VALIDATOR/lib/go";
GOROOT="\$HOME_VALIDATOR/.go"
APP_BIN="\$HOME_VALIDATOR/bin"
EOF
```

Install Gravity Bridge and GBT

```bash
wget -O bin/gravityd https://github.com/Gravity-Bridge/Gravity-Bridge/releases/download/v1.7.2/gravity-linux-amd64
wget -O bin/gbt https://github.com/Gravity-Bridge/Gravity-Bridge/releases/download/v1.7.2/gbt
chmod +x bin/*
```

Create Systemd

```bash
sudo tee /etc/systemd/system/gravityd.service > /dev/null << EOF
[Unit]
Description=gravitybridge node service
After=network-online.target

[Service]
User=salinem
Group=salinem
ExecStart=/mainnet/salinem/bin/gravityd start --home $HOME/.gravity
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF

sudo systemctl daemon-reload
sudo systemctl enable gravityd
```

Reload shell

```bash
source ~/.zshrc
```

Initialized Node

{% hint style="info" %}
YOUR NAME NODE IS MONIKER
{% endhint %}

```bash
gravityd init "YOUR NAME NODE" --chain-id gravity-bridge-3
```

Download Genesis

```bash
wget -c https://github.com/roomit-xyz/Mainnet-Validator/blob/main/Gravity-Bridge/config/genesis.json?raw=true -O genesis.json && mv genesis.json  $HOME/.gravity/config/
wget -c 
https://raw.githubusercontent.com/roomit-xyz/Mainnet-Validator/main/Gravity-Bridge/config/addrbook.json && mv addrbook.json 
$HOME/.gravity/config/
```

Edit config, You can refer to [edit-configuration-node.md](../../gravity-bridge/installation-node/edit-configuration-node.md "mention")

Apply State Sync, refer to [state-sync.md](../../gravity-bridge/installation-node/state-sync.md "mention")
