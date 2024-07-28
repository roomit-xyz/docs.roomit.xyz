---
description: Genesis Public Empe From RoomIT
---

### Genesis

{%  embed url="https://roomit.xyz/genesis/testnet/empe/" %}


### How to Deploy

Stop Your Service Blockchain
```bash
sudo systemctl stop empe-node
```
>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop emped__


Download And Deploy
```
curl -Ls  https://roomit.xyz/genesis/testnet/empe/genesis.json > $HOME/.empe-chain/config/genesis.json 
```

Start Your Service Blockchain
```bash
sudo systemctl start empe-node
```