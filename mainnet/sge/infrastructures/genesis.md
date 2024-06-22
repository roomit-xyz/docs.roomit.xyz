---
description: Genesis Public Sge From RoomIT
---

### Genesis

{%  embed url="https://roomit.xyz/genesis/mainnet/sge/" %}


### How to Deploy

Stop Your Service Blockchain
```bash
sudo systemctl stop sge-node
```
>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop sged__


Download And Deploy
```
curl -Ls  https://roomit.xyz/genesis/mainnet/sge/genesis.json > $HOME/.sge/config/genesis.json 
```

Start Your Service Blockchain
```bash
sudo systemctl start sge-node
```