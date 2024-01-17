---
description: Netfilter Iptables
---

# Firewall Block

```
######## Block VALIDATOR
## gRPC
-A INPUT -s 10.66.66.0/24 -p tcp -m multiport --dports 1307 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# gRPC-web
-A INPUT -s 10.66.66.0/24 -p tcp -m multiport --dports 1407 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# api
-A INPUT -s 10.66.66.0/24  -p tcp -m multiport --dports 1207 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# rpc
-A INPUT -s 10.66.66.0/24 -p tcp -m multiport --dports 16707 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# p2p
-A INPUT -p tcp -m multiport --dports 16607 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Metrics
-A INPUT -s 10.66.66.0/24 -p tcp -m multiport --dports 16807 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT

```
