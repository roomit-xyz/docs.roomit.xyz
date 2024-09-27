---
description: Genesis Public Arkeo From RoomIT
---

### Genesis

{%  embed url="https://roomit.xyz/genesis/testnet/arkeo/" %}


### How to Deploy

Stop Your Service Blockchain
```bash
sudo systemctl stop arkeo-node
```
>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop arkeod__


Download And Deploy
```
curl -Ls  https://roomit.xyz/genesis/testnet/arkeo/genesis.json > $HOME/.arkeo/config/genesis.json 
```

Start Your Service Blockchain
```bash
sudo systemctl start arkeo-node
```