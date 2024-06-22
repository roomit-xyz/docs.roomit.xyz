---
description: Port Mapping And Firewall Sge |  RoomIT Validator
---


# Port Mapping

### EXAMPLE
| Services           | Port  | Protocol | Status          |
| ------------------ | ----- | -------- | --------------- |
| Proxy App          | 16505 | TCP      | Private         |
| RPC                | 16705 | TCP      | Private         |
| pProf              | 1105  | TCP      | Private         |
| P2P                | 16605 | TCP      | Public (Direct) |
| Prometheus Metrics | 16805 | TCP      | Private         |
| API                | 1205  | TCP      | Private         |
| gRPC               | 1305  | TCP      | Private         |
| gRPC Web           | 1405 | TCP      | Private         |
| TMKMS              | 5005 | TCP      | Private         |


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

