---
description: AddressBook Planq From RoomIT
---

{%  embed url="https://roomit.xyz/addressbook/mainnet/planq/" %}
Planq Addressbook
{%  endembed %}

### How to Deploy

Stop Your Service Blockchain
```bash
sudo systemctl stop planq-node
```
>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop planqd__


Download And Deploy
```
curl -Ls  https://roomit.xyz/addressbook/mainnet/planq/addrbook.json > $HOME/.planqd/config/addrbook.json 
```

Start Your Service Blockchain
```bash
sudo systemctl start planq-node
```
