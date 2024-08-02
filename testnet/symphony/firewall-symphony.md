---
description: Port Mapping And Firewall Symphony |  RoomIT Validator
---


# Port Mapping

### EXAMPLE
| Services           | Port  | Protocol | Status          |
| ------------------ | ----- | -------- | --------------- |
| Proxy App          | 16511 | TCP      | Private         |
| RPC                | 16711 | TCP      | Private         |
| pProf              | 1111  | TCP      | Private         |
| P2P                | 16611 | TCP      | Public (Direct) |
| Prometheus Metrics | 16811 | TCP      | Private         |
| API                | 1211  | TCP      | Private         |
| gRPC               | 1311  | TCP      | Private         |
| gRPC Web           | 1411 | TCP      | Private         |
| TMKMS              | 5011 | TCP      | Private         |


# Firewall Symphony

```
######## Symphony Protocol VALIDATOR
# gRPC
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports  1311  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Horcrux / tmkms
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports  5011 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Consensus Metrics Exporter
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 1605 -j ACCEPT
# api
-A INPUT -s 10.xxx.xxx.xxx  -p tcp -m multiport --dports  1211  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# rpc
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 16711 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# p2p
-A INPUT -p tcp -m multiport --dports 16611  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Metrics
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 16811  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT

```

