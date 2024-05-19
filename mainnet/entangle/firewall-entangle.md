---
description: Netfilter Iptables
---

# Firewall Entangle

```
######## Entangle VALIDATOR
## gRPC
-A INPUT -s 10.66.66.0/24 -p tcp -m multiport --dports 1308 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# gRPC-web
-A INPUT -s 10.66.66.0/24 -p tcp -m multiport --dports 1408 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# api
-A INPUT -s 10.66.66.0/24  -p tcp -m multiport --dports 1208 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# rpc
-A INPUT -s 10.66.66.0/24 -p tcp -m multiport --dports 16708 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# p2p
-A INPUT -p tcp -m multiport --dports 16608 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Metrics
-A INPUT -s 10.66.66.0/24 -p tcp -m multiport --dports 16808 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT

```
