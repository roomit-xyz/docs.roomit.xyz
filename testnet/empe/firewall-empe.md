---
description: Port Mapping And Firewall Empe |  RoomIT Validator
---


# Port Mapping

### EXAMPLE
| Services           | Port  | Protocol | Status          |
| ------------------ | ----- | -------- | --------------- |
| Proxy App          | 16510 | TCP      | Private         |
| RPC                | 16710 | TCP      | Private         |
| pProf              | 1110  | TCP      | Private         |
| P2P                | 16610 | TCP      | Public (Direct) |
| Prometheus Metrics | 16810 | TCP      | Private         |
| API                | 1210  | TCP      | Private         |
| gRPC               | 1310  | TCP      | Private         |
| gRPC Web           | 1410 | TCP      | Private         |
| TMKMS              | 5010 | TCP      | Private         |


# Firewall Empe

```
######## Empe Protocol VALIDATOR
# gRPC
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports  1310  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Horcrux / tmkms
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports  5010 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Consensus Metrics Exporter
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 1605 -j ACCEPT
# api
-A INPUT -s 10.xxx.xxx.xxx  -p tcp -m multiport --dports  1210  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# rpc
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 16710 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# p2p
-A INPUT -p tcp -m multiport --dports 16610  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Metrics
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 16810  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT

```

