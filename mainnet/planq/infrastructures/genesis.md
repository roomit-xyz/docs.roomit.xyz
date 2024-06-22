---
description: Genesis Public Planq From RoomIT
---

### Genesis

{%  embed url="https://roomit.xyz/genesis/mainnet/planq/" %}


### How to Deploy

Stop Your Service Blockchain
```bash
sudo systemctl stop planq-node
```
>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop planqd__


Download And Deploy
```
curl -Ls  https://roomit.xyz/genesis/mainnet/planq/genesis.json > $HOME/.planqd/config/genesis.json 
```

Start Your Service Blockchain
```bash
sudo systemctl start planq-node
```