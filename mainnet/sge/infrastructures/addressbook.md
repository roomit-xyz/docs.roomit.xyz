---
description: AddressBook Sge From RoomIT
---

{%  embed url="https://roomit.xyz/addressbook/mainnet/sge/" %}
Sge Addressbook
{%  endembed %}

### How to Deploy

Stop Your Service Blockchain
```bash
sudo systemctl stop sge-node
```
>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop sged__


Download And Deploy
```
curl -Ls  https://roomit.xyz/addressbook/mainnet/sge/addrbook.json > $HOME/.sge/config/addrbook.json 
```

Start Your Service Blockchain
```bash
sudo systemctl start sge-node
```
