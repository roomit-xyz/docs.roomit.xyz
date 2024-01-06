---
description: Netfilter Iptables
---

# Firewall Gitopia

```
######## GITOPIA VALIDATOR
# gRPC
-A INPUT -s 10.66.66.254,192.168.66.2 -p tcp -m multiport --dports 1301 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Horcrux
-A INPUT -s 10.66.66.251,10.66.66.252,10.66.66.253,192.168.66.3,192.168.66.4,192.168.66.5 -p tcp -m multiport --dports 5001 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Consensus Metrics Exporter
-A INPUT -s 10.66.66.254,192.168.66.2 -p tcp -m multiport --dports 1601 -j ACCEPT
# gRPC-web
-A INPUT -s 10.66.66.254,192.168.66.2 -p tcp -m multiport --dports 1401 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# api
-A INPUT -s 10.66.66.254,192.168.66.2  -p tcp -m multiport --dports 1201 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# rpc
-A INPUT -s 10.66.66.254,192.168.66.2 -p tcp -m multiport --dports 16701 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# p2p
-A INPUT -p tcp -m multiport --dports 16601 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Metrics
-A INPUT -s 10.66.66.254,192.168.66.2 -p tcp -m multiport --dports 16801 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT

```
