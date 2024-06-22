---
description: Port Mapping And Firewall Source |  RoomIT Validator
---


# Port Mapping

### EXAMPLE
| Services           | Port  | Protocol | Status          |
| ------------------ | ----- | -------- | --------------- |
| Proxy App          | 16502 | TCP      | Private         |
| RPC                | 16702 | TCP      | Private         |
| pProf              | 1102  | TCP      | Private         |
| P2P                | 16602 | TCP      | Public (Direct) |
| Prometheus Metrics | 16802 | TCP      | Private         |
| API                | 1202  | TCP      | Private         |
| gRPC               | 1302  | TCP      | Private         |
| gRPC Web           | 1402 | TCP      | Private         |
| TMKMS              | 5002 | TCP      | Private         |


# Firewall Source

```
######## Source Protocol VALIDATOR
# gRPC
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports  1302  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Horcrux / tmkms
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports  5002 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Consensus Metrics Exporter
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 1605 -j ACCEPT
# api
-A INPUT -s 10.xxx.xxx.xxx  -p tcp -m multiport --dports  1202  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# rpc
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 16702 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# p2p
-A INPUT -p tcp -m multiport --dports 16602  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Metrics
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 16802  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT

```

