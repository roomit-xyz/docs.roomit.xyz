---
description: Firewall Gravity Bridge
---

# Gravity Bridge

```bash
####### GRAVITY VALIDATOR
## gRPC
-A INPUT -s 10.xx.xx.0/24 -p tcp -m multiport --dports 5090 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# gRPC-web
-A INPUT -s 10.xx.xx.0/24 -p tcp -m multiport --dports 5091 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# api
-A INPUT -s 10.xx.xx.0/24  -p tcp -m multiport --dports 5317 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# rpc
-A INPUT -s 10.xx.xx.0/24 -p tcp -m multiport --dports 16657 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# p2p
-A INPUT -p tcp -m multiport --dports 16656 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Metrics
-A INPUT -s 10.xx.xx.0/24 -p tcp -m multiport --dports 16660 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
-A INPUT -s 10.xx.xx.0/24 -p tcp -m multiport --dports 6631 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT

```
