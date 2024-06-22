---
description: AddressBook Blockx From RoomIT
---

{%  embed url="https://roomit.xyz/addressbook/mainnet/blockx/" %}
Blockx Addressbook
{%  endembed %}

### How to Deploy

Stop Your Service Blockchain
```bash
sudo systemctl stop blockx-node
```
>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop blockxd__


Download And Deploy
```
curl -Ls  https://roomit.xyz/addressbook/mainnet/blockx/addrbook.json > $HOME/.blockxd/config/addrbook.json 
```

Start Your Service Blockchain
```bash
sudo systemctl start blockx-node
```
