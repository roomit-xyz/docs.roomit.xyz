---
description: AddressBook Fuelsequence From RoomIT
---

{%  embed url="https://roomit.xyz/addressbook/testnet/fuelsequence/" %}
Fuelsequence Addressbook
{%  endembed %}

### How to Deploy

Stop Your Service Blockchain
```bash
sudo systemctl stop fuelsequence-node
```
>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop fuelsequencerd__


Download And Deploy
```
curl -Ls  https://roomit.xyz/addressbook/testnet/fuelsequence/addrbook.json > $HOME/.fuelsequencer/config/addrbook.json 
```

Start Your Service Blockchain
```bash
sudo systemctl start fuelsequence-node
```
