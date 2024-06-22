---
description: Genesis Public Entangle From RoomIT
---

### Genesis

{%  embed url="https://roomit.xyz/genesis/mainnet/entangle/" %}


### How to Deploy

Stop Your Service Blockchain
```bash
sudo systemctl stop entangle-node
```
>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop entangled__


Download And Deploy
```
curl -Ls  https://roomit.xyz/genesis/mainnet/entangle/genesis.json > $HOME/.entangled/config/genesis.json 
```

Start Your Service Blockchain
```bash
sudo systemctl start entangle-node
```