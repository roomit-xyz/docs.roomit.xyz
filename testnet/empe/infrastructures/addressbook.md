---
description: AddressBook Empe From RoomIT
---

{%  embed url="https://roomit.xyz/addressbook/testnet/empe/" %}
Empe Addressbook
{%  endembed %}

### How to Deploy

Stop Your Service Blockchain
```bash
sudo systemctl stop empe-node
```
>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop emped__


Download And Deploy
```
curl -Ls  https://roomit.xyz/addressbook/testnet/empe/addrbook.json > $HOME/.empe-chain/config/addrbook.json 
```

Start Your Service Blockchain
```bash
sudo systemctl start empe-node
```
