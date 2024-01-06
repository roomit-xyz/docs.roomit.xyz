---
description: How to compile script bash
---

# Compile Script Bash

Download shc

```bash
 wget -c https://github.com/neurobin/shc/archive/refs/tags/4.0.3.zip -O shc.zip
 unzip shc.zip
 rm shc.zip
 mv shc-4.0.3 shc
```

Compile

```
cd shc
./configure
make
sudo make install
```

or you only want in /mainnet/salinem/bin justcopy binary

```
cp src/shc ~/bin
```

Compile Script [Broken link](broken-reference "mention")

```bash
shc -U -f auto-restake.sh -o auto-restake 
```
