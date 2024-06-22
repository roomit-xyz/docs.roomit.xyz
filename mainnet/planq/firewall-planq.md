---
description: Port Mapping And Firewall Planq |  RoomIT Validator
---


# Port Mapping

### EXAMPLE
| Services           | Port  | Protocol | Status          |
| ------------------ | ----- | -------- | --------------- |
| Proxy App          | 16503 | TCP      | Private         |
| RPC                | 16703 | TCP      | Private         |
| pProf              | 1103  | TCP      | Private         |
| P2P                | 16603 | TCP      | Public (Direct) |
| Prometheus Metrics | 16803 | TCP      | Private         |
| API                | 1203  | TCP      | Private         |
| gRPC               | 1303  | TCP      | Private         |
| gRPC Web           | 1403 | TCP      | Private         |
| TMKMS              | 5003 | TCP      | Private         |


# Firewall Planq

```
######## Planq Protocol VALIDATOR
# gRPC
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports  1303  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Horcrux / tmkms
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports  5003 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Consensus Metrics Exporter
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 1605 -j ACCEPT
# api
-A INPUT -s 10.xxx.xxx.xxx  -p tcp -m multiport --dports  1203  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# rpc
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 16703 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# p2p
-A INPUT -p tcp -m multiport --dports 16603  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Metrics
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 16803  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT

```

