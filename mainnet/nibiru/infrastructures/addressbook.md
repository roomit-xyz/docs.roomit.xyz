---
description: AddressBook Nibiru From RoomIT
---

{%  embed url="https://roomit.xyz/addressbook/mainnet/nibiru/" %}
Nibiru Addressbook
{%  endembed %}

### How to Deploy

Stop Your Service Blockchain
```bash
sudo systemctl stop nibiru-node
```
>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop nibid__


Download And Deploy
```
curl -Ls  https://roomit.xyz/addressbook/mainnet/nibiru/addrbook.json > $HOME/.nibid/config/addrbook.json 
```

Start Your Service Blockchain
```bash
sudo systemctl start nibiru-node
```
