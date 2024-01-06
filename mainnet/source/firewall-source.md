---
description: Netfilter Iptables
---

# Firewall Source

```
######## Source Protocol VALIDATOR
# gRPC
-A INPUT -s 10.66.66.254,192.168.66.2 -p tcp -m multiport --dports 1302 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Horcrux
-A INPUT -s 10.66.66.251,10.66.66.252,10.66.66.253,192.168.66.3,192.168.66.4,192.168.66.5 -p tcp -m multiport --dports 5002 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Consensus Metrics Exporter
-A INPUT -s 10.66.66.254,192.168.66.2 -p tcp -m multiport --dports 1602 -j ACCEPT
# gRPC-web
-A INPUT -s 10.66.66.254,192.168.66.2 -p tcp -m multiport --dports 1402 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# api
-A INPUT -s 10.66.66.254,192.168.66.2  -p tcp -m multiport --dports 1202 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# rpc
-A INPUT -s 10.66.66.254,192.168.66.2 -p tcp -m multiport --dports 16702 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# p2p
-A INPUT -p tcp -m multiport --dports 16602 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Metrics
-A INPUT -s 10.66.66.254,192.168.66.2 -p tcp -m multiport --dports 16802 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT

```
