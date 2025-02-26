---
description: Port Mapping And Firewall Fuelsequence |  RoomIT Validator
---


# Port Mapping

### EXAMPLE
| Services           | Port  | Protocol | Status          |
| ------------------ | ----- | -------- | --------------- |
| Proxy App          | 16512 | TCP      | Private         |
| RPC                | 16712 | TCP      | Private         |
| pProf              | 1112  | TCP      | Private         |
| P2P                | 16612 | TCP      | Public (Direct) |
| Prometheus Metrics | 16812 | TCP      | Private         |
| API                | 1212  | TCP      | Private         |
| gRPC               | 1312  | TCP      | Private         |
| gRPC Web           | 1412 | TCP      | Private         |
| TMKMS              | 5012 | TCP      | Private         |


# Firewall Fuelsequence

```
######## Fuelsequence Protocol VALIDATOR
# gRPC
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports  1312  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Horcrux / tmkms
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports  5012 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Consensus Metrics Exporter
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 1605 -j ACCEPT
# api
-A INPUT -s 10.xxx.xxx.xxx  -p tcp -m multiport --dports  1212  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# rpc
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 16712 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# p2p
-A INPUT -p tcp -m multiport --dports 16612  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Metrics
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 16812  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT

```

