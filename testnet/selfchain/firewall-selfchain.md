---
description: Port Mapping And Firewall Selfchain |  RoomIT Validator
---


# Port Mapping

### EXAMPLE
| Services           | Port  | Protocol | Status          |
| ------------------ | ----- | -------- | --------------- |
| Proxy App          | 26503 | TCP      | Private         |
| RPC                | 26703 | TCP      | Private         |
| pProf              | 2103  | TCP      | Private         |
| P2P                | 26603 | TCP      | Public (Direct) |
| Prometheus Metrics | 26803 | TCP      | Private         |
| API                | 2203  | TCP      | Private         |
| gRPC               | 2303  | TCP      | Private         |
| gRPC Web           | 2403 | TCP      | Private         |
| TMKMS              | 5003 | TCP      | Private         |


# Firewall Selfchain

```
######## Selfchain Protocol VALIDATOR
# gRPC
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports  2303  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Horcrux / tmkms
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports  5003 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Consensus Metrics Exporter
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 1605 -j ACCEPT
# api
-A INPUT -s 10.xxx.xxx.xxx  -p tcp -m multiport --dports  2203  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# rpc
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 26703 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# p2p
-A INPUT -p tcp -m multiport --dports 26603  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Metrics
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 26803  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT

```

