---
description: Netfilter Iptables
---

# Firewall Gravity Bridge

```
######## GRAVITY VALIDATOR
## gRPC
-A INPUT -s 10.66.66.0/24 -p tcp -m multiport --dports 1300 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# gRPC-web
-A INPUT -s 10.66.66.0/24 -p tcp -m multiport --dports 1400 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# api
-A INPUT -s 10.66.66.0/24  -p tcp -m multiport --dports 1200 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# rpc
-A INPUT -s 10.66.66.0/24 -p tcp -m multiport --dports 16700 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# p2p
-A INPUT -p tcp -m multiport --dports 16600 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Metrics
-A INPUT -s 10.66.66.0/24 -p tcp -m multiport --dports 16800 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
-A INPUT -s 10.66.66.0/24 -p tcp -m multiport --dports 1500 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
```
