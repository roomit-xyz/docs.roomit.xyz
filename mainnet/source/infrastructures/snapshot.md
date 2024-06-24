---
description: Snapshot Source From RoomIT
---



## SNAPSHOT Source From RoomIT


### How to Deploy Snapshot?


1. Install Compression/Decompression Tools
```bash
sudo apt install lzma -y
```

2. Download Snapshot
```bash
SNAPSHOT=$(curl -s https://roomit.xyz/snapshot/mainnet/source/ | grep -i "<a href=" | grep lzma | grep -v md5sum | awk -F"=" '{print $2}' |  sed 's/"//g' | sed "s/>//g" | sed "s/ //g")
wget -c https://roomit.xyz/snapshot/mainnet/source/${SNAPSHOT} --inet4-only
```

3. Stop Your Service Blockchain
```bash
sudo systemctl stop source-node
```
>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop sourced__

4. Backup State Validator
```bash
cp ${HOME}/.source/data/priv_validator_state.json ${HOME}/.source/priv_validator_state.json
```

4. Reset Node Blockchain
```bash
sourced tendermint unsafe-reset-all --home $HOME/.source --keep-addr-book
```

5. Extract Data Snapshot
```bash
lzma -d -c ${SNAPSHOT} | tar -xv -C $HOME/.source 
```

6. Restore State Validator
```bash
cp ${HOME}/.source /priv_validator_state.json ${HOME}/.source/data/priv_validator_state.json
```

7. Start Node Blockchain
```bash
sudo systemctl start source-node
```