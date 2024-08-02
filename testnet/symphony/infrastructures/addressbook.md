---
description: AddressBook Symphony From RoomIT
---

{%  embed url="https://roomit.xyz/addressbook/testnet/symphony/" %}
Symphony Addressbook
{%  endembed %}

### How to Deploy

Stop Your Service Blockchain
```bash
sudo systemctl stop symphony-node
```
>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop symphonyd__


Download And Deploy
```
curl -Ls  https://roomit.xyz/addressbook/testnet/symphony/addrbook.json > $HOME/.symphonyd/config/addrbook.json 
```

Start Your Service Blockchain
```bash
sudo systemctl start symphony-node
```
