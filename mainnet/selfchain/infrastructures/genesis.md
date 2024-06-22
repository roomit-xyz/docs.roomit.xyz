---
description: Genesis Public Selfchain From RoomIT
---

### Genesis

{%  embed url="https://roomit.xyz/genesis/mainnet/selfchain/" %}


### How to Deploy

Stop Your Service Blockchain
```bash
sudo systemctl stop selfchain-node
```
>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop selfchaind__


Download And Deploy
```
curl -Ls  https://roomit.xyz/genesis/mainnet/selfchain/genesis.json > $HOME/.selfchain/config/genesis.json 
```

Start Your Service Blockchain
```bash
sudo systemctl start selfchain-node
```