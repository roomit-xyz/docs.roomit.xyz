---
description: Comdex Netfilter Iptables
---

# Firewall - Comdex

```bash
######## COMDEX VALIDATOR
## gRPC
-A INPUT -s 10.66.66.0/24 -p tcp -m multiport --dports 1301 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# gRPC-web
-A INPUT -s 10.66.66.0/24 -p tcp -m multiport --dports 1401 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# api
-A INPUT -s 10.66.66.0/24  -p tcp -m multiport --dports 1201 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# rpc
-A INPUT -s 10.66.66.0/24 -p tcp -m multiport --dports 16701 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# p2p
-A INPUT -p tcp -m multiport --dports 16601 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Metrics
-A INPUT -s 10.66.66.0/24 -p tcp -m multiport --dports 16801 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT

```
