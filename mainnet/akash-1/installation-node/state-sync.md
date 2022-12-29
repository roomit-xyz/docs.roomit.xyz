---
description: >-
  With our state sync services you will be able to catch up latest chain block
  in matter of minutes
---

# Snapshot

{% hint style="info" %}
**Running in user** : _salinem_
{% endhint %}

Remove Existing data

```
rm -rf ~/.akash/data; \
mkdir -p ~/.akash/data; \
cd ~/.akash/data
```

Download Snapshot

```
SNAP_URL="$(curl -s -o - https://cosmos-snapshots.s3.filebase.com/akash/pruned/snapshot.json | jq -r '.latest')"

wget -c "$SNAP_URL"

SNAP="${SNAP_URL##*/}"

tar xzvf "$SNAP" --force-local
```
