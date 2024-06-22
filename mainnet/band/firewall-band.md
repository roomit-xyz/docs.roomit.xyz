---
description: Port Mapping And Firewall Band |  RoomIT Validator
---


# Port Mapping

### EXAMPLE
| Services           | Port  | Protocol | Status          |
| ------------------ | ----- | -------- | --------------- |
| Proxy App          | 16500 | TCP      | Private         |
| RPC                | 16700 | TCP      | Private         |
| pProf              | 1100  | TCP      | Private         |
| P2P                | 16600 | TCP      | Public (Direct) |
| Prometheus Metrics | 16800 | TCP      | Private         |
| API                | 1200  | TCP      | Private         |
| gRPC               | 1300  | TCP      | Private         |
| gRPC Web           | 1400 | TCP      | Private         |
| TMKMS              | 5000 | TCP      | Private         |


# Firewall Band

```
######## Band Protocol VALIDATOR
# gRPC
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports  1300  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Horcrux / tmkms
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports  5000 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Consensus Metrics Exporter
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 1605 -j ACCEPT
# api
-A INPUT -s 10.xxx.xxx.xxx  -p tcp -m multiport --dports  1200  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# rpc
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 16700 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# p2p
-A INPUT -p tcp -m multiport --dports 16600  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Metrics
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 16800  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT

```

