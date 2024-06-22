---
description: Genesis Public Dvpn From RoomIT
---

### Genesis

{%  embed url="https://roomit.xyz/genesis/mainnet/dvpn/" %}


### How to Deploy

Stop Your Service Blockchain
```bash
sudo systemctl stop dvpn-node
```
>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop sentinelhub__


Download And Deploy
```
curl -Ls  https://roomit.xyz/genesis/mainnet/dvpn/genesis.json > $HOME/.sentinelhub/config/genesis.json 
```

Start Your Service Blockchain
```bash
sudo systemctl start dvpn-node
```