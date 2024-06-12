---
description: Netfilter Iptables
---

# Firewall Sge

```
######## Sge Protocol VALIDATOR
# gRPC
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports  1305  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Horcrux / tmkms
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports  5005 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Consensus Metrics Exporter
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 1605 -j ACCEPT
# api
-A INPUT -s 10.xxx.xxx.xxx  -p tcp -m multiport --dports  1205  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# rpc
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 16705 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# p2p
-A INPUT -p tcp -m multiport --dports 16605  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Metrics
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 16805  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT

```

