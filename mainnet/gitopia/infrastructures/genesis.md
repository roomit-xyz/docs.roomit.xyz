---
description: Genesis Public Gitopia From RoomIT
---

### Genesis

{%  embed url="https://roomit.xyz/genesis/mainnet/gitopia/" %}


### How to Deploy

Stop Your Service Blockchain
```bash
sudo systemctl stop gitopia-node
```
>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop gitopiad__


Download And Deploy
```
curl -Ls  https://roomit.xyz/genesis/mainnet/gitopia/genesis.json > $HOME/.gitopia/config/genesis.json 
```

Start Your Service Blockchain
```bash
sudo systemctl start gitopia-node
```