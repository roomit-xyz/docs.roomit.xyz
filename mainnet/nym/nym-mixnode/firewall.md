---
description: Net Filter NYM Mixnode
---

# Firewall

```
### GLOBAL
-A INPUT -m state --state RELATED,ESTABLISHED -j ACCEPT
-A INPUT -i lo -j ACCEPT


### NYM
-A INPUT  -p tcp -m tcp -m state --state NEW,RELATED,ESTABLISHED  --dport 1789 -j ACCEPT
-A INPUT  -p tcp -m tcp -m state --state NEW,RELATED,ESTABLISHED  --dport 1790 -j ACCEPT
-A INPUT  -p tcp -m tcp -m state --state NEW,RELATED,ESTABLISHED  --dport 8000 -j ACCEPT

```
