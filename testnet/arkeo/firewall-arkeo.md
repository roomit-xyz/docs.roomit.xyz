---
description: Port Mapping And Firewall Arkeo |  RoomIT Validator
---


# Port Mapping

### EXAMPLE
| Services           | Port  | Protocol | Status          |
| ------------------ | ----- | -------- | --------------- |
| Proxy App          | 26500 | TCP      | Private         |
| RPC                | 26700 | TCP      | Private         |
| pProf              | 2100  | TCP      | Private         |
| P2P                | 26600 | TCP      | Public (Direct) |
| Prometheus Metrics | 26800 | TCP      | Private         |
| API                | 2200  | TCP      | Private         |
| gRPC               | 2300  | TCP      | Private         |
| gRPC Web           | 2400 | TCP      | Private         |
| TMKMS              | 5000 | TCP      | Private         |


# Firewall Arkeo

```
######## Arkeo Protocol VALIDATOR
# gRPC
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports  2300  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Horcrux / tmkms
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports  5000 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Consensus Metrics Exporter
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 1605 -j ACCEPT
# api
-A INPUT -s 10.xxx.xxx.xxx  -p tcp -m multiport --dports  2200  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# rpc
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 26700 -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# p2p
-A INPUT -p tcp -m multiport --dports 26600  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT
# Metrics
-A INPUT -s 10.xxx.xxx.xxx -p tcp -m multiport --dports 26800  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT

```

