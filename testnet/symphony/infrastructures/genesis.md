---
description: Genesis Public Symphony From RoomIT
---

### Genesis

{%  embed url="https://roomit.xyz/genesis/testnet/symphony/" %}


### How to Deploy

Stop Your Service Blockchain
```bash
sudo systemctl stop symphony-node
```
>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop symphonyd__


Download And Deploy
```
curl -Ls  https://roomit.xyz/genesis/testnet/symphony/genesis.json > $HOME/.symphonyd/config/genesis.json 
```

Start Your Service Blockchain
```bash
sudo systemctl start symphony-node
```