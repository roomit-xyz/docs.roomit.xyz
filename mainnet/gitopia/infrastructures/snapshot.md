---
description: Snapshot Gitopia From RoomIT
---



## SNAPSHOT Gitopia From RoomIT


### How to Deploy Snapshot?


1. Install Compression/Decompression Tools
```bash
sudo apt install lzma -y
```

2. Download Snapshot
```bash
SNAPSHOT=$(curl -s https://roomit.xyz/snapshot/mainnet/gitopia/ | grep -i "<a href=" | grep lzma | grep -v md5sum | awk -F"=" '{print $2}' |  sed 's/"//g' | sed "s/>//g" | sed "s/ //g")
wget -c https://roomit.xyz/snapshot/mainnet/gitopia/${SNAPSHOT} --inet4-only
```

3. Stop Your Service Blockchain
```bash
sudo systemctl stop gitopia-node
```
>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop gitopiad__

4. Backup State Validator
```bash
cp ${HOME}/.gitopia/data/priv_validator_state.json ${HOME}/.gitopia/priv_validator_state.json
```

4. Reset Node Blockchain
```bash
gitopiad tendermint unsafe-reset-all --home $HOME/.gitopia --keep-addr-book
```

5. Extract Data Snapshot
```bash
lzma -d -c ${SNAPSHOT} | tar -xv -C $HOME/.gitopia 
```

6. Restore State Validator
```bash
cp ${HOME}/.gitopia /priv_validator_state.json ${HOME}/.gitopia /data/priv_validator_state.json
```

7. Start Node Blockchain
```bash
sudo systemctl start gitopia-node
```