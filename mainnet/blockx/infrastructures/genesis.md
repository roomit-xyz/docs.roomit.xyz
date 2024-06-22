---
description: Genesis Public Blockx From RoomIT
---

### Genesis

{%  embed url="https://roomit.xyz/genesis/mainnet/blockx/" %}


### How to Deploy

Stop Your Service Blockchain
```bash
sudo systemctl stop blockx-node
```
>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop blockxd__


Download And Deploy
```
curl -Ls  https://roomit.xyz/genesis/mainnet/blockx/genesis.json > $HOME/.blockxd/config/genesis.json 
```

Start Your Service Blockchain
```bash
sudo systemctl start blockx-node
```