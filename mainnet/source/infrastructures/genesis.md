---
description: Genesis Public Source From RoomIT
---

### Genesis

{%  embed url="https://roomit.xyz/genesis/mainnet/source/" %}


### How to Deploy

Stop Your Service Blockchain
```bash
sudo systemctl stop source-node
```
>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop sourced__


Download And Deploy
```
curl -Ls  https://roomit.xyz/genesis/mainnet/source/genesis.json > $HOME/.source/config/genesis.json 
```

Start Your Service Blockchain
```bash
sudo systemctl start source-node
```