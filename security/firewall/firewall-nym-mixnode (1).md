---
description: Netfilter Iptables
---

# Firewall NYM Mixnode

```
######### NYM MIXNODE
-A INPUT -i enp0s31f6   -p tcp -m tcp -m state --state NEW,RELATED,ESTABLISHED  --dport 1789 -j ACCEPT
-A INPUT -i mainnet-02  -p tcp -m tcp -m state --state NEW,RELATED,ESTABLISHED  --dport 1789 -j ACCEPT
-A INPUT -i enp0s31f6   -p tcp -m tcp -m state --state NEW,RELATED,ESTABLISHED  --dport 1790 -j ACCEPT
-A INPUT -i mainnet-02  -p tcp -m tcp -m state --state NEW,RELATED,ESTABLISHED  --dport 1790 -j ACCEPT
-A INPUT -i enp0s31f6   -p tcp -m tcp -m state --state NEW,RELATED,ESTABLISHED  --dport 8000 -j ACCEPT
-A INPUT -i mainnet-02  -p tcp -m tcp -m state --state NEW,RELATED,ESTABLISHED  --dport 8000 -j ACCEPT
-A INPUT  -s 10.66.66.0/24 -p tcp -m multiport --dports 9101 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT

```
