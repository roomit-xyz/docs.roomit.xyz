---
description: Port Mapping And Firewall Selfchain |  RoomIT Validator
---


# Port Mapping

### EXAMPLE
| Services           | Port  | Protocol | Status          |
| ------------------ | ----- | -------- | --------------- |
| Proxy App          | 16509 | TCP      | Private         |
| RPC                | 16709 | TCP      | Private         |
| pProf              | 1109  | TCP      | Private         |
| P2P                | 16609 | TCP      | Public (Direct) |
| Prometheus Metrics | 16809 | TCP      | Private         |
| API                | 1209  | TCP      | Private         |
| gRPC               | 1309  | TCP      | Private         |
| gRPC Web           | 1409 | TCP      | Private         |
| TMKMS              | 5009 | TCP      | Private         |


# Firewall Selfchain

```
######## Selfchain Protocol VALIDATOR
# gRPC
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports  1309  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Horcrux / tmkms
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports  5009 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Consensus Metrics Exporter
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 1605 -j ACCEPT
# api
-A INPUT -s 10.xxx.xxx.xxx  -p tcp -m multiport --dports  1209  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# rpc
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 16709 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# p2p
-A INPUT -p tcp -m multiport --dports 16609  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Metrics
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 16809  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT

```

