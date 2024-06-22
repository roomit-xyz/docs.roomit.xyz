---
description: Genesis Public Band From RoomIT
---

### Genesis

{%  embed url="https://roomit.xyz/genesis/mainnet/band/" %}


### How to Deploy

Stop Your Service Blockchain
```bash
sudo systemctl stop band-node
```
>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop bandd__


Download And Deploy
```
curl -Ls  https://roomit.xyz/genesis/mainnet/band/genesis.json > $HOME/.band/config/genesis.json 
```

Start Your Service Blockchain
```bash
sudo systemctl start band-node
```