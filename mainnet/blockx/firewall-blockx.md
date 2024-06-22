---
description: Port Mapping And Firewall Blockx |  RoomIT Validator
---


# Port Mapping

### EXAMPLE
| Services           | Port  | Protocol | Status          |
| ------------------ | ----- | -------- | --------------- |
| Proxy App          | 16507 | TCP      | Private         |
| RPC                | 16707 | TCP      | Private         |
| pProf              | 1107  | TCP      | Private         |
| P2P                | 16607 | TCP      | Public (Direct) |
| Prometheus Metrics | 16807 | TCP      | Private         |
| API                | 1207  | TCP      | Private         |
| gRPC               | 1307  | TCP      | Private         |
| gRPC Web           | 1407 | TCP      | Private         |
| TMKMS              | 5007 | TCP      | Private         |


# Firewall Blockx

```
######## Blockx Protocol VALIDATOR
# gRPC
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports  1307  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Horcrux / tmkms
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports  5007 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Consensus Metrics Exporter
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 1605 -j ACCEPT
# api
-A INPUT -s 10.xxx.xxx.xxx  -p tcp -m multiport --dports  1207  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# rpc
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 16707 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# p2p
-A INPUT -p tcp -m multiport --dports 16607  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Metrics
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 16807  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT

```

