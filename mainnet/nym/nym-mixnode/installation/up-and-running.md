---
description: First Running Container Mixnode
---

# Up And Running

{% hint style="info" %}
Notes : Setup Mixnode must be Finish First
{% endhint %}

Build Images

```bash
bash run.sh build mixnode
```

Run Container

```bash
bash run.sh container mixnode
```

Check Container&#x20;

```bash
docker ps
```

Stop Container&#x20;

```bash
docker stop nym-mixnode-server 
```

Assign Wallet address in /mainnet/salinem/.env\_mixnode. For get Wallet Address

```bash
 cat /salinem/mainnet/data/mixnodes/"YOUR ID MIXNODE"/config/config.toml | grep wallet_address
```

Start again&#x20;

```bash
docker start nym-mixnode-server 
```

Check Logs

```bash
docker logs -f nym-mixnode-server 
```
