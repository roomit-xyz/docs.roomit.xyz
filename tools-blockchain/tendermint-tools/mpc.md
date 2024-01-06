---
description: Multi Party Computation
---

# 🟢 Horcrux MPC

Example deploy MPC for PlanQ Blockchain

### Main Horcrux

In Parent Work Directory, make sure you have 2 file, following :

* script deploy horcrux
* priv\_validator\_key.json

Script Horcrux-Deploy

```bash
#!/bin/bash
PORT_VALIDATOR="5001"
PORT_RPC_VALIDATOR="16701"
HORCRUX_METRICS="1601"
BLOCKCHAIN="gitopia"
CHAIN_ID="gitopia"
HOME_BLOCKCHAIN="/horcrux/${BLOCKCHAIN}"
PRIVATE_KEY_HOME="${HOME_BLOCKCHAIN}/conf"
URL_DOWNLOAD_BIN="https://github.com/strangelove-ventures/horcrux/releases/download/v3.1.0/horcrux_linux-amd64"
TOTAL_NODE=2


if ! getent passwd "$1" >/dev/null; then
    echo "User $1 not found."
    useradd -m -d ${HOME_BLOCKCHAIN} -s /bin/bash ${BLOCKCHAIN}
else
    echo "User $1 exists."
fi

if ! [ -f /usr/local/bin/horcrux ]
then
   wget -c ${URL_DOWNLOAD_BIN}
   cp horcrux_linux-amd64 /usr/local/bin
fi

if ! [ -f  /etc/profile.d/horcrux.sh ]
then
    echo "export PATH="${PATH}:/usr/local/bin"" >> /etc/profile.d/horcrux.sh
    source /etc/profile.d/horcrux.sh
fi


mkdir -p $HOME_BLOCKCHAIN/systemd ${HOME_BLOCKCHAIN}/conf/shard
if ! [ -f $(pwd)/priv_validator_key.json ]
then
    echo $(pwd)/priv_validator_key.json not exsist
    exit 1;
fi
cp priv_validator_key.json ${PRIVATE_KEY_HOME}/priv_validator_key.json
chown ${BLOCKCHAIN}:${BLOCKCHAIN} -R $HOME_BLOCKCHAIN/systemd ${HOME_BLOCKCHAIN}/conf/shard  ${PRIVATE_KEY_HOME}/priv_validator_key.json 
usermod -aG app ${BLOCKCHAIN}
if [ ${TOTAL_NODE} == 2 ]
then
  su  - ${BLOCKCHAIN} -c "cd ${HOME_BLOCKCHAIN}/conf/shard ; horcrux config init --node "tcp://10.66.66.2:${PORT_VALIDATOR}" --node "tcp://192.168.66.18:${PORT_VALIDATOR}" \
                    --cosigner "tcp://192.168.66.3:${PORT_RPC_VALIDATOR}" --cosigner "tcp://192.168.66.4:${PORT_RPC_VALIDATOR}"  \
                    --threshold 2 --grpc-timeout 1000ms --raft-timeout 1000ms --debug-addr 0.0.0.0:${HORCRUX_METRICS}"
  su  - ${BLOCKCHAIN} -c "cd ${HOME_BLOCKCHAIN}/conf/shard ; horcrux create-ecies-shards --shards 2"
  su  - ${BLOCKCHAIN} -c "cd ${HOME_BLOCKCHAIN}/conf/shard ;horcrux create-ed25519-shards --chain-id ${CHAIN_ID} --key-file ${PRIVATE_KEY_HOME}/priv_validator_key.json --threshold 2 --shards 2"
elif [ ${TOTAL_NODE} == 3 ]
then
  su  - ${BLOCKCHAIN} -c "cd ${HOME_BLOCKCHAIN}/conf/shard ; horcrux config init --node "tcp://10.66.66.2:${PORT_VALIDATOR}" --node "tcp://192.168.66.18:${PORT_VALIDATOR}" \
                    --cosigner "tcp://192.168.66.3:${PORT_RPC_VALIDATOR}" --cosigner "tcp://192.168.66.4:${PORT_RPC_VALIDATOR}" --cosigner "tcp://192.168.66.5:${PORT_RPC_VALIDATOR}" \
                    --threshold 3 --grpc-timeout 1000ms --raft-timeout 1000ms --debug-addr 0.0.0.0:${HORCRUX_METRICS}"
  su  - ${BLOCKCHAIN} -c "cd ${HOME_BLOCKCHAIN}/conf/shard ; horcrux create-ecies-shards --shards 3"
  su  - ${BLOCKCHAIN} -c "cd ${HOME_BLOCKCHAIN}/conf/shard ;horcrux create-ed25519-shards --chain-id ${CHAIN_ID} --key-file ${PRIVATE_KEY_HOME}/priv_validator_key.json --threshold 3 --shards 3"
else
  echo "Sorry, Setup Autmation horcrux only 2 - 3"
  exit 1;
fi
read -p  "Make Sure Stop Yoour node validator and show data/priv_validator_state.json"
su  - ${BLOCKCHAIN} -c "horcrux state import data"
su  - ${BLOCKCHAIN} -c "cat > ${HOME_BLOCKCHAIN}/systemd/horcrux-${BLOCKCHAIN}.service<<EOF
[Unit]
Description=MPC Signer node
After=network.target

[Service]
Type=simple
User=${BLOCKCHAIN}
WorkingDirectory=${HOME_BLOCKCHAIN}
ExecStart=/usr/local/bin/horcrux start 
Restart=on-failure
RestartSec=3
LimitNOFILE=4096

[Install]
WantedBy=multi-user.target
EOF
"
ln  -sf ${HOME_BLOCKCHAIN}/systemd/horcrux-${BLOCKCHAIN}.service /etc/systemd/system/horcrux-${BLOCKCHAIN}.service
systemctl daemon-reload
systemctl enable ${HOME_BLOCKCHAIN}/systemd/horcrux-${BLOCKCHAIN}.service
```

Private Validator

```
{
  "address": "",
  "pub_key": {
    "type": "tendermint/PubKeyEd25519",
    "value": ""
  },
  "priv_key": {
    "type": "tendermint/PrivKeyEd25519",
    "value": ""
  }
}
```

How to deploy?

```
bash horcrux-deploy
```

Copy and Send All Key to ${HOME}/.horcrux

### Worker Horcrux

Script Horcrux Deploy For Worker

```bash
#!/bin/bash
PORT_VALIDATOR="5001"
PORT_RPC_VALIDATOR="16701"
HORCRUX_METRICS="1601"
BLOCKCHAIN="gitopia"
CHAIN_ID="gitopia"
HOME_BLOCKCHAIN="/horcrux/${BLOCKCHAIN}"
PRIVATE_KEY_HOME="${HOME_BLOCKCHAIN}/conf"
URL_DOWNLOAD_BIN="https://github.com/strangelove-ventures/horcrux/releases/download/v3.1.0/horcrux_linux-amd64"
TOTAL_NODE=2


if ! getent passwd "$1" >/dev/null; then
    echo "User $1 not found."
    useradd -m -d ${HOME_BLOCKCHAIN} -s /bin/bash ${BLOCKCHAIN}
else
    echo "User $1 exists."
fi

if ! [ -f /usr/local/bin/horcrux ]
then
   wget -c ${URL_DOWNLOAD_BIN}
   cp horcrux_linux-amd64 /usr/local/bin
fi

if ! [ -f  /etc/profile.d/horcrux.sh ]
then
    echo "export PATH="${PATH}:/usr/local/bin"" >> /etc/profile.d/horcrux.sh
    source /etc/profile.d/horcrux.sh
fi


mkdir -p $HOME_BLOCKCHAIN/systemd ${HOME_BLOCKCHAIN}/conf/shard
if ! [ -f $(pwd)/priv_validator_key.json ]
then
    echo $(pwd)/priv_validator_key.json not exsist
    exit 1;
fi
cp priv_validator_key.json ${PRIVATE_KEY_HOME}/priv_validator_key.json
chown ${BLOCKCHAIN}:${BLOCKCHAIN} -R $HOME_BLOCKCHAIN/systemd ${HOME_BLOCKCHAIN}/conf/shard  ${PRIVATE_KEY_HOME}/priv_validator_key.json 
usermod -aG app ${BLOCKCHAIN}
if [ ${TOTAL_NODE} == 2 ]
then
  su  - ${BLOCKCHAIN} -c "cd ${HOME_BLOCKCHAIN}/conf/shard ; horcrux config init --node "tcp://10.66.66.2:${PORT_VALIDATOR}" --node "tcp://192.168.66.18:${PORT_VALIDATOR}" \
                    --cosigner "tcp://192.168.66.3:${PORT_RPC_VALIDATOR}" --cosigner "tcp://192.168.66.4:${PORT_RPC_VALIDATOR}"  \
                    --threshold 2 --grpc-timeout 1000ms --raft-timeout 1000ms --debug-addr 0.0.0.0:${HORCRUX_METRICS}"
elif [ ${TOTAL_NODE} == 3 ]
then
  su  - ${BLOCKCHAIN} -c "cd ${HOME_BLOCKCHAIN}/conf/shard ; horcrux config init --node "tcp://10.66.66.2:${PORT_VALIDATOR}" --node "tcp://192.168.66.18:${PORT_VALIDATOR}" \
                    --cosigner "tcp://192.168.66.3:${PORT_RPC_VALIDATOR}" --cosigner "tcp://192.168.66.4:${PORT_RPC_VALIDATOR}" --cosigner "tcp://192.168.66.5:${PORT_RPC_VALIDATOR}" \
                    --threshold 3 --grpc-timeout 1000ms --raft-timeout 1000ms --debug-addr 0.0.0.0:${HORCRUX_METRICS}"
else
  echo "Sorry, Setup Autmation horcrux only 2 - 3"
  exit 1;
fi
read -p  "Make Sure Stop Yoour node validator and show data/priv_validator_state.json"
su  - ${BLOCKCHAIN} -c "horcrux state import data"
su  - ${BLOCKCHAIN} -c "cat > ${HOME_BLOCKCHAIN}/systemd/horcrux-${BLOCKCHAIN}.service<<EOF
[Unit]
Description=MPC Signer node
After=network.target

[Service]
Type=simple
User=${BLOCKCHAIN}
WorkingDirectory=${HOME_BLOCKCHAIN}
ExecStart=/usr/local/bin/horcrux start 
Restart=on-failure
RestartSec=3
LimitNOFILE=4096

[Install]
WantedBy=multi-user.target
EOF
"
ln  -sf ${HOME_BLOCKCHAIN}/systemd/horcrux.service /etc/systemd/system/horcrux-${BLOCKCHAIN}.service
systemctl daemon-reload
systemctl enable ${HOME_BLOCKCHAIN}/systemd/horcrux-${BLOCKCHAIN}.service

```
