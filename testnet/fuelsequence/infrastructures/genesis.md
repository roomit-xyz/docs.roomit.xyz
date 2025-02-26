---
description: Genesis Public Fuelsequence From RoomIT
---

### Genesis

{%  embed url="https://roomit.xyz/genesis/testnet/fuelsequence/" %}


### How to Deploy

Stop Your Service Blockchain
```bash
sudo systemctl stop fuelsequence-node
```
>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop fuelsequencerd__


Download And Deploy
```
curl -Ls  https://roomit.xyz/genesis/testnet/fuelsequence/genesis.json > $HOME/.fuelsequencer/config/genesis.json 
```

Start Your Service Blockchain
```bash
sudo systemctl start fuelsequence-node
```