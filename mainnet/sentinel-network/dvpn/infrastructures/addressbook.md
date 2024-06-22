---
description: AddressBook Dvpn From RoomIT
---

{%  embed url="https://roomit.xyz/addressbook/mainnet/dvpn/" %}
Dvpn Addressbook
{%  endembed %}

### How to Deploy

Stop Your Service Blockchain
```bash
sudo systemctl stop dvpn-node
```
>> Notes : for systemd init blockchain, adjust with your name of service. __systemctl stop sentinelhub__


Download And Deploy
```
curl -Ls  https://roomit.xyz/addressbook/mainnet/dvpn/addrbook.json > $HOME/.sentinelhub/config/addrbook.json 
```

Start Your Service Blockchain
```bash
sudo systemctl start dvpn-node
```
