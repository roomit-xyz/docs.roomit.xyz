---
description: Firewall Gravity Bridge
---

# Gravity Bridge

```bash
####### GITOPIA VALIDATOR
## gRPC
-A INPUT -p tcp -m multiport --dports 2301 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# gRPC-web
-A INPUT -p tcp -m multiport --dports 2401 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# api
-A INPUT -s 10.66.66.0/24,127.0.0.1  -p tcp -m multiport --dports 2201 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# rpc
-A INPUT -s 10.66.66.0/24,127.0.0.1 -p tcp -m multiport --dports 26701 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# p2p
-A INPUT -p tcp -m multiport --dports 26601 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Metrics
-A INPUT -s 10.66.66.0/24,127.0.0.1  -p tcp -m multiport --dports 26801 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT

```
