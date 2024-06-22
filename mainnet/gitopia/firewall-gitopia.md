---
description: Port Mapping And Firewall Gitopia |  RoomIT Validator
---


# Port Mapping

### EXAMPLE
| Services           | Port  | Protocol | Status          |
| ------------------ | ----- | -------- | --------------- |
| Proxy App          | 16501 | TCP      | Private         |
| RPC                | 16701 | TCP      | Private         |
| pProf              | 1101  | TCP      | Private         |
| P2P                | 16601 | TCP      | Public (Direct) |
| Prometheus Metrics | 16801 | TCP      | Private         |
| API                | 1201  | TCP      | Private         |
| gRPC               | 1301  | TCP      | Private         |
| gRPC Web           | 1401 | TCP      | Private         |
| TMKMS              | 5001 | TCP      | Private         |


# Firewall Gitopia

```
######## Gitopia Protocol VALIDATOR
# gRPC
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports  1301  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Horcrux / tmkms
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports  5001 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Consensus Metrics Exporter
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 1605 -j ACCEPT
# api
-A INPUT -s 10.xxx.xxx.xxx  -p tcp -m multiport --dports  1201  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# rpc
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 16701 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# p2p
-A INPUT -p tcp -m multiport --dports 16601  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Metrics
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 16801  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT

```

