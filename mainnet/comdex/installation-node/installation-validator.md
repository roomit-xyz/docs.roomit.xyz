---
description: Install Node Producer
---

# Installation Validator

**Chain ID**: comdex-1 | **Latest Binary Version**: v6.0.2

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
mkdir -p ${HOME}/conf
```

Install Golang

<pre class="language-bash"><code class="lang-bash">GOLANG_VERSION="1.19.4"
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

Install Comdex&#x20;

```bash
cd "${HOME}/lib"
git clone https://github.com/comdex-official/comdex.git
cd comdex
git fetch --tags
git checkout v6.0.2
unset GOPATH
export GOPATH="${HOME}/lib"
go mod vendor
make install
cp ${HOME}/lib/go/bin/comdex ${HOME}/bin
```

Check version, ensure you have version v6.0.2

```
comdex version
```

Setup Shell Variable, add line in your shell profile ${HOME}/.zshrc or if using bash ${HOME}/.bashrc

```bash
####### COMDEX ######
export PATH="${PATH}:/${HOME}/bin"
source /app/comdex/conf/cosmovisor-env
```

Initialized Node

{% hint style="info" %}
YOUR NAME NODE IS MONIKER
{% endhint %}

```bash
comdex init "YOUR NAME NODE" --chain-id comdex-1
```

Download Genesis

```bash
curl https://raw.githubusercontent.com/comdex-official/networks/main/mainnet/comdex-1/genesis.json > ~/.comdex/config/genesis.json
```

Edit config, You can refer to [edit-configuration-node.md](edit-configuration-node.md "mention")

Apply State Sync, refer to [state-sync.md](state-sync.md "mention")
