---
description: AddressBook Atomone From RoomIT
---

{%  embed url="https://roomit.xyz/addressbook/mainnet/atomone/" %}
Atomone Addressbook
{%  endembed %}

### How to Deploy

Stop Your Service Blockchain
```bash
sudo systemctl stop atomone-node
```
>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop atomoned__


Download And Deploy
```
curl -Ls  https://roomit.xyz/addressbook/mainnet/atomone/addrbook.json > $HOME/.atomone/config/addrbook.json 
```

Start Your Service Blockchain
```bash
sudo systemctl start atomone-node
```
