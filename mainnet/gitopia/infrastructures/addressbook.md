---
description: AddressBook Gitopia From RoomIT
---

{%  embed url="https://roomit.xyz/addressbook/mainnet/gitopia/" %}
Gitopia Addressbook
{%  endembed %}

### How to Deploy

Stop Your Service Blockchain
```bash
sudo systemctl stop gitopia-node
```
>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop gitopiad__


Download And Deploy
```
curl -Ls  https://roomit.xyz/addressbook/mainnet/gitopia/addrbook.json > $HOME/.gitopia/config/addrbook.json 
```

Start Your Service Blockchain
```bash
sudo systemctl start gitopia-node
```
