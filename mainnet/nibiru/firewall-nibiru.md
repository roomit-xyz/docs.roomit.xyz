---
description: Port Mapping And Firewall Nibiru |  RoomIT Validator
---


# Port Mapping

### EXAMPLE
| Services           | Port  | Protocol | Status          |
| ------------------ | ----- | -------- | --------------- |
| Proxy App          | 16506 | TCP      | Private         |
| RPC                | 16706 | TCP      | Private         |
| pProf              | 1106  | TCP      | Private         |
| P2P                | 16606 | TCP      | Public (Direct) |
| Prometheus Metrics | 16806 | TCP      | Private         |
| API                | 1206  | TCP      | Private         |
| gRPC               | 1306  | TCP      | Private         |
| gRPC Web           | 1406 | TCP      | Private         |
| TMKMS              | 5006 | TCP      | Private         |


# Firewall Nibiru

```
######## Nibiru Protocol VALIDATOR
# gRPC
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports  1306  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Horcrux / tmkms
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports  5006 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Consensus Metrics Exporter
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 1605 -j ACCEPT
# api
-A INPUT -s 10.xxx.xxx.xxx  -p tcp -m multiport --dports  1206  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# rpc
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 16706 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# p2p
-A INPUT -p tcp -m multiport --dports 16606  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Metrics
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 16806  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT

```

