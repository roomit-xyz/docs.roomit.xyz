---
description: AddressBook Crossfi From RoomIT
---

{%  embed url="https://roomit.xyz/addressbook/mainnet/crossfi/" %}
Crossfi Addressbook
{%  endembed %}

### How to Deploy

Stop Your Service Blockchain
```bash
sudo systemctl stop crossfi-node
```
>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop mineplex-chaind__


Download And Deploy
```
curl -Ls  https://roomit.xyz/addressbook/mainnet/crossfi/addrbook.json > $HOME/.mineplex-chain/config/addrbook.json 
```

Start Your Service Blockchain
```bash
sudo systemctl start crossfi-node
```
