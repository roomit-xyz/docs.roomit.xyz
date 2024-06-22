---
description: Snapshot Dvpn From RoomIT
---



## SNAPSHOT Dvpn From RoomIT


### How to Deploy Snapshot?


1. Install Compression/Decompression Tools
```bash
sudo apt install lzma -y
```

2. Download Snapshot
```bash
SNAPSHOT=$(curl -s https://roomit.xyz/snapshot/mainnet/dvpn/ | grep -i "<a href=" | grep lzma | grep -v md5sum | awk -F"=" '{print $2}' |  sed 's/"//g' | sed "s/>//g" | sed "s/ //g")
wget -c https://roomit.xyz/snapshot/mainnet/dvpn/${SNAPSHOT} --inet4-only
```

3. Stop Your Service Blockchain
```bash
sudo systemctl stop dvpn-node
```
>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop sentinelhub__

4. Backup State Validator
```bash
cp ${HOME}/.sentinelhub/data/priv_validator_state.json ${HOME}/.sentinelhub/priv_validator_state.json
```

4. Reset Node Blockchain
```bash
sentinelhub tendermint unsafe-reset-all --home $HOME/.sentinelhub --keep-addr-book
```

5. Extract Data Snapshot
```bash
lzma -d -c ${SNAPSHOT} | tar -xv -C $HOME/.sentinelhub 
```

6. Restore State Validator
```bash
cp ${HOME}/.sentinelhub /priv_validator_state.json ${HOME}/.sentinelhub /data/priv_validator_state.json
```

7. Start Node Blockchain
```bash
sudo systemctl start dvpn-node
```