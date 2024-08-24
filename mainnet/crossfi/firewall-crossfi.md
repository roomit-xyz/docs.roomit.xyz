---
description: Port Mapping And Firewall Crossfi |  RoomIT Validator
---


# Port Mapping

### EXAMPLE
| Services           | Port  | Protocol | Status          |
| ------------------ | ----- | -------- | --------------- |
| Proxy App          | 26510 | TCP      | Private         |
| RPC                | 26710 | TCP      | Private         |
| pProf              | 2110  | TCP      | Private         |
| P2P                | 26610 | TCP      | Public (Direct) |
| Prometheus Metrics | 26810 | TCP      | Private         |
| API                | 2210  | TCP      | Private         |
| gRPC               | 2310  | TCP      | Private         |
| gRPC Web           | 2410 | TCP      | Private         |
| TMKMS              | 5010 | TCP      | Private         |


# Firewall Crossfi

```
######## Crossfi Protocol VALIDATOR
# gRPC
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports  2310  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Horcrux / tmkms
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports  5010 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Consensus Metrics Exporter
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 1605 -j ACCEPT
# api
-A INPUT -s 10.xxx.xxx.xxx  -p tcp -m multiport --dports  2210  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# rpc
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 26710 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# p2p
-A INPUT -p tcp -m multiport --dports 26610  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Metrics
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 26810  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT

```

