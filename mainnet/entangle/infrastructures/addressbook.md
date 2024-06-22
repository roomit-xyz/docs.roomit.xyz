---
description: AddressBook Entangle From RoomIT
---

{%  embed url="https://roomit.xyz/addressbook/mainnet/entangle/" %}
Entangle Addressbook
{%  endembed %}

### How to Deploy

Stop Your Service Blockchain
```bash
sudo systemctl stop entangle-node
```
>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop entangled__


Download And Deploy
```
curl -Ls  https://roomit.xyz/addressbook/mainnet/entangle/addrbook.json > $HOME/.entangled/config/addrbook.json 
```

Start Your Service Blockchain
```bash
sudo systemctl start entangle-node
```
