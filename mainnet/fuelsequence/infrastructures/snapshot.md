---
description: Snapshot Fuelsequence From RoomIT
---



## SNAPSHOT Fuelsequence From RoomIT


### How to Deploy Snapshot?


1. Install Compression/Decompression Tools
```bash
sudo apt install lzma -y
```

2. Download Snapshot
```bash
SNAPSHOT=$(curl -s https://roomit.xyz/snapshot/mainnet/fuelsequence/ | grep -i "<a href=" | grep lzma | grep -v md5sum | awk -F"=" '{print $2}' |  sed 's/"//g' | sed "s/>//g" | sed "s/ //g")
wget -c https://roomit.xyz/snapshot/mainnet/fuelsequence/${SNAPSHOT} --inet4-only
```

3. Stop Your Service Blockchain
```bash
sudo systemctl stop fuelsequence-node
```
>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop fuelsequencerd__

4. Backup State Validator
```bash
cp ${HOME}/.fuelsequencer/data/priv_validator_state.json ${HOME}/.fuelsequencer/priv_validator_state.json
```

4. Reset Node Blockchain
```bash
fuelsequencerd tendermint unsafe-reset-all --home $HOME/.fuelsequencer --keep-addr-book
```

5. Extract Data Snapshot
```bash
lzma -d -c ${SNAPSHOT} | tar -xv -C $HOME/.fuelsequencer 
```

6. Restore State Validator
```bash
cp ${HOME}/.fuelsequencer /priv_validator_state.json ${HOME}/.fuelsequencer/data/priv_validator_state.json
```

7. Start Node Blockchain
```bash
sudo systemctl start fuelsequence-node
```