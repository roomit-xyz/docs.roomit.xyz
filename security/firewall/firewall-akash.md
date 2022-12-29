---
description: Netfilter Iptables
---

# Firewall Akash

```
######## AKASH VALIDATOR
## gRPC
-A INPUT -s 10.66.66.0/24 -p tcp -m multiport --dports 1302 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# gRPC-web
-A INPUT -s 10.66.66.0/24 -p tcp -m multiport --dports 1402 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# api
-A INPUT -s 10.66.66.0/24  -p tcp -m multiport --dports 1202 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# rpc
-A INPUT -s 10.66.66.0/24 -p tcp -m multiport --dports 16702 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# p2p
-A INPUT -p tcp -m multiport --dports 16602 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Metrics
-A INPUT -s 10.66.66.0/24 -p tcp -m multiport --dports 16802 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT

```
