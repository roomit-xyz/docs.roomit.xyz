---
description: Port Mapping And Firewall Dvpn |  RoomIT Validator
---


# Port Mapping

### EXAMPLE
| Services           | Port  | Protocol | Status          |
| ------------------ | ----- | -------- | --------------- |
| Proxy App          | 16504 | TCP      | Private         |
| RPC                | 16704 | TCP      | Private         |
| pProf              | 1104  | TCP      | Private         |
| P2P                | 16604 | TCP      | Public (Direct) |
| Prometheus Metrics | 16804 | TCP      | Private         |
| API                | 1204  | TCP      | Private         |
| gRPC               | 1304  | TCP      | Private         |
| gRPC Web           | 1404 | TCP      | Private         |
| TMKMS              | 5004 | TCP      | Private         |


# Firewall Dvpn

```
######## Dvpn Protocol VALIDATOR
# gRPC
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports  1304  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Horcrux / tmkms
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports  5004 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Consensus Metrics Exporter
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 1605 -j ACCEPT
# api
-A INPUT -s 10.xxx.xxx.xxx  -p tcp -m multiport --dports  1204  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# rpc
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 16704 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# p2p
-A INPUT -p tcp -m multiport --dports 16604  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Metrics
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 16804  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT

```

