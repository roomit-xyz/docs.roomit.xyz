---
description: Tendermint Puning Storage
---

# Pruning Storage Tendermint

<pre class="language-bash"><code class="lang-bash"><strong>#!/bin/bash
</strong>#
# RoomIT
# https://roomit.xyz
# If this script useful and you will visit cikarang indonesia, 
# Let's drink coffee and talk about blockchain
#


CONF_VALIDATOR=""
pruning="custom"
pruning_keep_recent="100"
pruning_keep_every="0"
pruning_interval="10"
indexer="null"

sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" ${HOME}/${CONF_VALIDATOR}/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" ${HOME}/${CONF_VALIDATOR}/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" ${HOME}/${CONF_VALIDATOR}/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" ${HOME}/${CONF_VALIDATOR}/config/app.toml
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" ${HOME}/${CONF_VALIDATOR}/config/config.toml

</code></pre>
