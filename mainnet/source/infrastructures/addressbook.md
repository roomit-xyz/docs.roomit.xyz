---
description: AddressBook Source From RoomIT
---

{%  embed url="https://roomit.xyz/addressbook/mainnet/source/" %}
Source Addressbook
{%  endembed %}

### How to Deploy

Stop Your Service Blockchain
```bash
sudo systemctl stop source-node
```
>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop sourced__


Download And Deploy
```
curl -Ls  https://roomit.xyz/addressbook/mainnet/source/addrbook.json > $HOME/.source/config/addrbook.json 
```

Start Your Service Blockchain
```bash
sudo systemctl start source-node
```
