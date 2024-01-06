---
description: Authz Config
---

# 🟢 Authz

### Restake Authz&#x20;

\
on file restake/src/networks.local.json

```
{
  "gravitybridge": {
    "prettyName": "gravitybridge",
    "restUrl": [
      "https://api.gravity.roomit.xyz"
    ]
  },
  "8ball": {
    "prettyName": "8ball",
    "restUrl": [
      "https://api.8ball.roomit.xyz"
    ]
  },
  "planq": {
    "prettyName": "planq",
    "restUrl": [
      "https://api.planq.roomit.xyz/"
    ],
    "gasPrice": "30000000000aplanq",
    "autostake": {
      "correctSlip44": true,
      "retries": 3,
      "batchPageSize": 100,
      "batchQueries": 25,
      "batchTxs": 50,
      "delegationsTimeout": 20000,
      "queryTimeout": 5000,
      "queryThrottle": 100,
      "gasModifier": 1.1
    }
  }
}


```

### Registry Validator

on file chains.json

```
{
  "$schema": "../chains.schema.json",
  "name": "RoomIT",
  "chains": [
   {
      "name": "gravitybridge",
      "address": "gravityvaloper1ssduj8c0cc8kquljvw3ygq9hduvcysnf590lmz",
      "restake": {
        "address": "gravity10lhel2eexpz2j6uam8klr64uacg9rcqhj6wc9f",
        "run_time": "every 12 hour",
        "minimum_reward": 1000000
      }
   },
   "name": "planq",
   "address": "plqvaloper1fqnr328nlndkxek2jaz8teec0euyr5yh26q26l",
   "restake": {
      "address": "plq15nsufxhvu56xyp2ngzsprslj5w57jn6ry3rtch",
      "run_time": "19:00",
      "minimum_reward": 100
       }
   },
   {
   "name":"8ball",
   "address":"8ballvaloper1999ptelp4fw8u9pgy5w2z2wpsy96xy3k2cg0uu",
   "restake":{
       "address":"8ball10lhel2eexpz2j6uam8klr64uacg9rcqh7ludr7",
       "run_time":"every 12 hour",
       "minimum_reward":100
       }
   }
  ]
}

```
