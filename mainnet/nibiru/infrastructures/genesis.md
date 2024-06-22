---
description: Genesis Public Nibiru From RoomIT
---

### Genesis

{%  embed url="https://roomit.xyz/genesis/mainnet/nibiru/" %}


### How to Deploy

Stop Your Service Blockchain
```bash
sudo systemctl stop nibiru-node
```
>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop nibid__


Download And Deploy
```
curl -Ls  https://roomit.xyz/genesis/mainnet/nibiru/genesis.json > $HOME/.nibid/config/genesis.json 
```

Start Your Service Blockchain
```bash
sudo systemctl start nibiru-node
```