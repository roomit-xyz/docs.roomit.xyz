---
description: Port Mapping And Firewall Entangle |  RoomIT Validator
---


# Port Mapping

### EXAMPLE
| Services           | Port  | Protocol | Status          |
| ------------------ | ----- | -------- | --------------- |
| Proxy App          | 16508 | TCP      | Private         |
| RPC                | 16708 | TCP      | Private         |
| pProf              | 1108  | TCP      | Private         |
| P2P                | 16608 | TCP      | Public (Direct) |
| Prometheus Metrics | 16808 | TCP      | Private         |
| API                | 1208  | TCP      | Private         |
| gRPC               | 1308  | TCP      | Private         |
| gRPC Web           | 1408 | TCP      | Private         |
| TMKMS              | 5008 | TCP      | Private         |


# Firewall Entangle

```
######## Entangle Protocol VALIDATOR
# gRPC
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports  1308  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Horcrux / tmkms
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports  5008 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Consensus Metrics Exporter
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 1605 -j ACCEPT
# api
-A INPUT -s 10.xxx.xxx.xxx  -p tcp -m multiport --dports  1208  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# rpc
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 16708 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# p2p
-A INPUT -p tcp -m multiport --dports 16608  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Metrics
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 16808  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT

```

