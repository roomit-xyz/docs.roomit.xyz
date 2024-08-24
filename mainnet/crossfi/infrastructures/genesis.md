---
description: Genesis Public Crossfi From RoomIT
---

### Genesis

{%  embed url="https://roomit.xyz/genesis/mainnet/crossfi/" %}


### How to Deploy

Stop Your Service Blockchain
```bash
sudo systemctl stop crossfi-node
```
>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop mineplex-chaind__


Download And Deploy
```
curl -Ls  https://roomit.xyz/genesis/mainnet/crossfi/genesis.json > $HOME/.mineplex-chain/config/genesis.json 
```

Start Your Service Blockchain
```bash
sudo systemctl start crossfi-node
```