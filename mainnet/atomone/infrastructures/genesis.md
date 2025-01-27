---
description: Genesis Public Atomone From RoomIT
---

### Genesis

{%  embed url="https://roomit.xyz/genesis/mainnet/atomone/" %}


### How to Deploy

Stop Your Service Blockchain
```bash
sudo systemctl stop atomone-node
```
>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop atomoned__


Download And Deploy
```
curl -Ls  https://roomit.xyz/genesis/mainnet/atomone/genesis.json > $HOME/.atomone/config/genesis.json 
```

Start Your Service Blockchain
```bash
sudo systemctl start atomone-node
```