---
description: Snapshot Selfchain From RoomIT
---



## SNAPSHOT Selfchain From RoomIT


### How to Deploy Snapshot?


1. Install Compression/Decompression Tools
```bash
sudo apt install zstd -y
```

2. Download Snapshot
```bash
SNAPSHOT=$(curl -s https://roomit.xyz/snapshot/testnet/selfchain/ | grep -i "<a href=" | grep zst | grep -v md5sum | awk -F"=" '{print $2}' |  sed 's/"//g' | sed "s/>//g" | sed "s/ //g")
wget -c https://roomit.xyz/snapshot/testnet/selfchain/${SNAPSHOT} --inet4-only
```

3. Stop Your Service Blockchain
```bash
sudo systemctl stop selfchain-node
```
>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop selfchaind__

4. Backup State Validator
```bash
cp ${HOME}/.selfchain/data/priv_validator_state.json ${HOME}/.selfchain/priv_validator_state.json
```

4. Reset Node Blockchain
```bash
selfchaind tendermint unsafe-reset-all --home $HOME/.selfchain --keep-addr-book
```

5. Extract Data Snapshot
```bash
zstd -d ${SNAPSHOT} -c | tar -xv -C  $HOME/.selfchain 
```

6. Restore State Validator
```bash
cp ${HOME}/.selfchain /priv_validator_state.json ${HOME}/.selfchain/data/priv_validator_state.json
```

7. Start Node Blockchain
```bash
sudo systemctl start selfchain-node
```