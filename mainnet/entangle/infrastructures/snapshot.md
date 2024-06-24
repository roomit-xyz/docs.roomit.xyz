---
description: Snapshot Entangle From RoomIT
---



## SNAPSHOT Entangle From RoomIT


### How to Deploy Snapshot?


1. Install Compression/Decompression Tools
```bash
sudo apt install lzma -y
```

2. Download Snapshot
```bash
SNAPSHOT=$(curl -s https://roomit.xyz/snapshot/mainnet/entangle/ | grep -i "<a href=" | grep lzma | grep -v md5sum | awk -F"=" '{print $2}' |  sed 's/"//g' | sed "s/>//g" | sed "s/ //g")
wget -c https://roomit.xyz/snapshot/mainnet/entangle/${SNAPSHOT} --inet4-only
```

3. Stop Your Service Blockchain
```bash
sudo systemctl stop entangle-node
```
>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop entangled__

4. Backup State Validator
```bash
cp ${HOME}/.entangled/data/priv_validator_state.json ${HOME}/.entangled/priv_validator_state.json
```

4. Reset Node Blockchain
```bash
entangled tendermint unsafe-reset-all --home $HOME/.entangled --keep-addr-book
```

5. Extract Data Snapshot
```bash
lzma -d -c ${SNAPSHOT} | tar -xv -C $HOME/.entangled 
```

6. Restore State Validator
```bash
cp ${HOME}/.entangled /priv_validator_state.json ${HOME}/.entangled/data/priv_validator_state.json
```

7. Start Node Blockchain
```bash
sudo systemctl start entangle-node
```