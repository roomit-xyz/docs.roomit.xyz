---
description: Snapshot Empe From RoomIT
---



## SNAPSHOT Empe From RoomIT


### How to Deploy Snapshot?


1. Install Compression/Decompression Tools
```bash
sudo apt install lzma -y
```

2. Download Snapshot
```bash
SNAPSHOT=$(curl -s https://roomit.xyz/snapshot/testnet/empe/ | grep -i "<a href=" | grep lzma | grep -v md5sum | awk -F"=" '{print $2}' |  sed 's/"//g' | sed "s/>//g" | sed "s/ //g")
wget -c https://roomit.xyz/snapshot/testnet/empe/${SNAPSHOT} --inet4-only
```

3. Stop Your Service Blockchain
```bash
sudo systemctl stop empe-node
```
>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop emped__

4. Backup State Validator
```bash
cp ${HOME}/.empe-chain/data/priv_validator_state.json ${HOME}/.empe-chain/priv_validator_state.json
```

4. Reset Node Blockchain
```bash
emped tendermint unsafe-reset-all --home $HOME/.empe-chain --keep-addr-book
```

5. Extract Data Snapshot
```bash
lzma -d -c ${SNAPSHOT} | tar -xv -C $HOME/.empe-chain 
```

6. Restore State Validator
```bash
cp ${HOME}/.empe-chain /priv_validator_state.json ${HOME}/.empe-chain/data/priv_validator_state.json
```

7. Start Node Blockchain
```bash
sudo systemctl start empe-node
```