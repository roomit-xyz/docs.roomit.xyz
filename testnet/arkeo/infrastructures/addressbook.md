---
description: AddressBook Arkeo From RoomIT
---

{%  embed url="https://roomit.xyz/addressbook/testnet/arkeo/" %}
Arkeo Addressbook
{%  endembed %}

### How to Deploy

Stop Your Service Blockchain
```bash
sudo systemctl stop arkeo-node
```
>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop arkeod__


Download And Deploy
```
curl -Ls  https://roomit.xyz/addressbook/testnet/arkeo/addrbook.json > $HOME/.arkeo/config/addrbook.json 
```

Start Your Service Blockchain
```bash
sudo systemctl start arkeo-node
```
