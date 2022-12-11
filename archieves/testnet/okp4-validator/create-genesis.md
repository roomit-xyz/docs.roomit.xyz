---
description: How to create genesis
---

# Create Genesis

{% hint style="info" %}
Assume, We run in user _salinem._ you can refer to [user-and-group-management.md](../../../security/user-and-group-management.md "mention")
{% endhint %}

Login to user salinem

```bash
sudo su - salinem
```

Create FHS

```bash
mkdir -p bin log lib systemd
```

Download Binary OKP4

```bash
wget -c https://github.com/okp4/okp4d/releases/download/v3.0.0/okp4d-3.0.0-linux-amd64
```

Move Binary to bin FHS

```
mv okp4d-3.0.0-linux-amd64 bin/okp4d
cd bin
ln -sf okp4d okp4
```

Init Moniker

```bash
okp4d init RoomIT  --home .okp4d
```

Create Wallet

```bash
okp4d keys add mywallet  --home .okp4d
```

Set Amount Necesery Balance

```bash
okp4d  add-genesis-account okp41zxxxxxxxxxxxxxxxx 10000200000uknow --home .okp4d
```

Generate Gentx

```bash
okp4d --home .okp4d gentx mywallet 10000000000uknow \                           
  --node-id $(okp4d --home .okp4d  tendermint show-node-id) \
  --chain-id okp4-nemeton-1 \
  --commission-rate 0.05 \
  --commission-max-rate 0.2 \
  --commission-max-change-rate 0.01 \
  --min-self-delegation 1 \
  --website "YOUR WEBSITE" \
  --details "YOUR NODE INFORMATION" \
  --identity "PGP KEYBASE" \
  --security-contact "EMAIL OF SECURITY"

```
