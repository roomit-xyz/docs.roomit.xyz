---
description: Rest API RoomIT
---

# Rest API Roomit

Roomit offers an API for seamless management and integration of blockchain services, enabling users to efficiently handle their investments and access network features and able integrating in our platform.

### Details

Host :
```
https://stake.roomit.xyz
```

#### Profile

* For Get details Our Validator Profiles

```
GET - /api/mainnet/profile
GET - /api/testnet/profile
```

Output :
```
........
[
  {
    "moniker": "Roomit",
    "valoper": "bandvaloper13tf84rkc9yflru6kty4xj8jtkuzl6fd66f3q3z",
    "wallet_address": "band13tf84rkc9yflru6kty4xj8jtkuzl6fd6kl4r5f",
    "blockchain_name": "band",
    "chain_id": "laozi-mainnet",
    "unit": "uband",
    "udecimal": 6,
    "coingeckoId": "band-protocol",
    "ibc": "not active",
    "ibc_wallet": null,
    "seeds": "08bc9afd0cac4ae6cf8f1877920b0cc7e58a6f42@seeds.tendermint.roomit.xyz:40000",
    "persistent": "420994846f175d0413796be9caea49e07ad3a503@p2p.tendermint.roomit.xyz:16600"
  }
]
.........
```

#### System

* For get version blockchain what we used
```
GET - /api/mainnet/version?blockchain_name=planq
```
Output :
```
{"version":"1.1.0"}
```

* For get last block from blockchain 
```
GET - /api/mainnet/lastblock?blockchain_name=planq
GET - /api/testnet/lastblock?blockchain_name=planq
```
Output :
```
{"latestBlockHeight":"9788147"}
```

* For Get Chain ID
```
GET - /api/mainnet/chainid?blockchain_name=planq
GET - /api/testnet/chainid?blockchain_name=planq
```
Output :
```
{"network":"planq_7070-2"}
```

* For Get Status Synchronize Block
```
GET - /api/mainnet/synced?blockchain_name=planq
GET - /api/testnet/synced?blockchain_name=planq
```
Output :
```
{"catchingUp":false}
```

  

#### Peers

* For Get Peers - Non Filtered from our RPC, this RPC no filtered only connected with our node will be show
```
GET - /api/mainnet/peers/nonfiltered?blockchain_name=planq
```
Output :
```
........
{"peers_nonfiltered":"6563c2409390cfea48623404812123315c5691df@undefined:26756,fb4cbb33179151e31311a498255f3194ced0c570@undefined:29656,3eb12284b7fb707490b8adfda6fa7d94e2fa5cd9@undefined:16603,76cc8a36e1ab54baef5dcb85663309daa494cee0@undefined:11156,97c53c39bb622da97a3aa4ab8cc6db32e67d6e8f@undefined:26656,89bd24ef4ccbd2425f127fd7822b4e55b0e09c36@undefined:26656,876b7a9b2b74ff7f6ba27028b24ca8aff5d9e9d9@undefined:61256,0e9387a4aa548998eda8f2bb4a5cd799345d5367@undefined:10756,b32cc55bf6382c9dbefb0c4ed5a615dbec1de41a@undefined:26656,fbf8a0d8dc3110f66e107752068346ecf8ef7eff@undefined:13656,6e6dd88c20bb7dc88caaefed5fa9bee59daa7a2c@undefined:20356,c8b05a87333041df65c0f61af47670e8811d8175@undefined:26656,97bf4b6f04f148edaa8c89d9033de470df9df5ca@undefined:26726,5086dee34ce6c69af7a6cf96d7d3a056d60c17ad@undefined:34656,f8bf62981f1c57b134a20e50d8afe016b1d0540b@undefined:26641,a3b8955aa523285d0aed51c7bfaf19eb20264ef5@undefined:10656,dd2f0ceaa0b21491ecae17413b242d69916550ae@undefined:26656,b039ad70d947e8b80b412310e7968eba370d5ffc@undefined:20356,e39dd27379e1a23760095a8f8d0943a30e264f5a@undefined:26726,8ad61ede4e3dc47b68b37f4c5309c11037d51b2f@undefined:26656,56f473a809cb87eaee37d9346a006e0b13077c50@undefined:26656,1efcb4b593bc4992ae413ac3c77f158a496908a4@undefined:26656,f0e3fa71a2eed651f0626fe913d9577a942f7e7a@undefined:33656,4d76396c180f8fab3abd34cc546f3c08c426905a@undefined:10656,fe5a38459191a00ad62e9f1913fc57581cef43a8@undefined:20356,5958a5508324f8bb1f7426c56937acd22e286577@undefined:30656,6e6415345f09527675e0d46d00c0a536c9c04f29@undefined:26656,ade4d8bc8cbe014af6ebdf3cb7b1e9ad36f412c0@undefined:20356,09ba537d6563018b97c502979c3478df4decf426@undefined:5656,bebfd590a2514520d158a2fce308e186e400f841@undefined:31656"}
.........
```

* For Get Peers - Filtered from our RPC, this RPC no filtered only connected with our node will be show
```
GET - /api/mainnet/peers/filtered?blockchain_name=planq
```
Output :
```
........
{"peers_filtered":"09ba537d6563018b97c502979c3478df4decf426@152.53.32.140:5656,0e9387a4aa548998eda8f2bb4a5cd799345d5367@65.21.198.100:10756,0facb3fba65c74a3f602a212b739d2db840a37d3@167.235.12.38:12156,106302ce8ae5c02dd16cc251f64950b03e58a96a@65.108.230.113:1176,17c83d5faba42f6a783684c67f94762b0809bbef@135.181.222.33:20356,1adb7bf1e2775981f20a6e7ae85d6c82d9c13ebc@65.109.104.72:20356,1efcb4b593bc4992ae413ac3c77f158a496908a4@49.13.209.160:26656,241c9414dd5654d7a5b39f438c4b4b70f4a93634@46.4.23.251:28656,29335bb56d235dc39e51d5a5e5f55717d81f00bd@34.215.64.62:26656,2daef0223a20a030076449e494397ef640a95c8e@5.189.136.141:26656,2f433bd6a566d32be7ec502699ccfefddd845c90@38.46.221.23:26656,30d0d17ec2c267bd3df6cb6e14cb9b0abee53054@65.109.62.179:46656,3374ff118b1f01b52ea6f52fe2914f11abfb2581@138.201.21.197:51656,3707bb058157f3a0fa7d97b481fbc8a3a9f8bdb9@65.109.94.250:26656,3eb12284b7fb707490b8adfda6fa7d94e2fa5cd9@65.109.99.157:16603,3fd002790baf7913921903b8c0b27f217088144f@185.190.140.93:14656,4ad2ca3628ec1bb5092fdef9fc31d763a9785093@31.220.75.198:26656,4d44b699af2c4b0a6b8dfdf266a8dc17765113db@85.239.238.23:11656,4d76396c180f8fab3abd34cc546f3c08c426905a@65.108.232.168:10656,5086dee34ce6c69af7a6cf96d7d3a056d60c17ad@94.130.138.48:34656,56e43c7abc96db9244dd5cc6292dc6bb5e2341c4@89.39.106.38:20356,56f473a809cb87eaee37d9346a006e0b13077c50@51.195.63.229:26656,5958a5508324f8bb1f7426c56937acd22e286577@46.4.23.225:30656,599baf73a8d8ae930e5ede9aa5bd92833fbd718c@51.159.213.195:26656,6354dd332572f8daf17f1a0b92e98120a0d5d91f@167.235.102.45:10656,6563c2409390cfea48623404812123315c5691df@65.109.93.152:26756,6e6415345f09527675e0d46d00c0a536c9c04f29@129.152.30.180:26656,6e6dd88c20bb7dc88caaefed5fa9bee59daa7a2c@147.135.31.22:20356,76cc8a36e1ab54baef5dcb85663309daa494cee0@65.108.196.251:11156,81e395d1305e609cf4dd47f07f70536cbccf8a2d@65.109.99.157:16603,876b7a9b2b74ff7f6ba27028b24ca8aff5d9e9d9@116.202.192.143:61256,880b6eacd765af80615517a59fec72788a4d7864@141.95.165.245:26656,88f3dafe5bf183be45836be03bb0e53b7c5ab4df@37.187.149.73:26706,89bd24ef4ccbd2425f127fd7822b4e55b0e09c36@194.163.150.181:26656,8ad61ede4e3dc47b68b37f4c5309c11037d51b2f@51.79.24.196:26656,9630583394c98c3495ec5752fa4c64952b2c0aa6@152.228.212.220:26656,97bf4b6f04f148edaa8c89d9033de470df9df5ca@147.135.138.180:26726,97c53c39bb622da97a3aa4ab8cc6db32e67d6e8f@146.59.110.50:26656,a1f6399349707fb752a4e23950861bf14569c843@167.235.9.223:26656,a3b8955aa523285d0aed51c7bfaf19eb20264ef5@152.53.18.245:10656,a8bbdc5c49cb5e2f7a7abb079332b2eeb0f2e182@104.37.185.178:26658,ade4d8bc8cbe014af6ebdf3cb7b1e9ad36f412c0@135.181.5.219:20356,b039ad70d947e8b80b412310e7968eba370d5ffc@148.113.1.83:20356,b32cc55bf6382c9dbefb0c4ed5a615dbec1de41a@93.81.241.55:26656,b62dca4c82918971ef12fcd25f0a2229e62595f6@213.246.39.138:28656,b95f231726d7ea85d7d000fa6a96b5118aa5f04a@46.165.204.15:27656,bebfd590a2514520d158a2fce308e186e400f841@65.108.66.174:31656,c8b05a87333041df65c0f61af47670e8811d8175@5.161.215.42:26656,c8b549e4982ec6ee75c4a2ff04c486aafd1d3722@5.78.114.77:26656,cfd0bf63d54ddb6222ae7a626c55b923529e8527@136.243.104.103:10756,dd2f0ceaa0b21491ecae17413b242d69916550ae@135.125.247.70:26656,e39dd27379e1a23760095a8f8d0943a30e264f5a@167.114.118.234:26726,e499c85df22319f64c2cea74ebd00d8ac2675844@167.235.14.83:14556,e7a2929fa8273d0aa0a83b2a25ad4fbdf4471558@212.227.73.190:29656,eb93bbbff168f16993da2c618c3ac6cebb09f9dd@184.174.35.79:26656,edf3c6acb49930a9b634bc609f601de0ebcbcf1a@81.0.221.144:36656,f0e3fa71a2eed651f0626fe913d9577a942f7e7a@65.108.237.188:33656,f7d55c947e3f9848d31b82a5562d9d2a3dd0e04c@65.109.24.78:41656,f8bf62981f1c57b134a20e50d8afe016b1d0540b@95.165.89.222:26641,fb4cbb33179151e31311a498255f3194ced0c570@65.108.127.231:29656,fbf8a0d8dc3110f66e107752068346ecf8ef7eff@65.108.226.120:13656,fdc59829040c6bcb20a76b44c2e4432cc2345c1e@45.55.135.162:10756,fe5a38459191a00ad62e9f1913fc57581cef43a8@65.108.238.29:20356,fe78937abf98c31f0ba3254146a5368412c78382@167.235.3.247:26636"}
.........
```

* For Get Our Seeds Peers
```
GET - /api/mainnet/peers/seeds?blockchain_name=planq
GET - /api/testnet/peers/seeds?blockchain_name=planq
```
Output :
```
{"seeds":"08bc9afd0cac4ae6cf8f1877920b0cc7e58a6f42@seeds.tendermint.roomit.xyz:40003"}
```

* For Get Our Persistent Peers
```
GET - /api/mainnet/peers/persistent?blockchain_name=planq
GET - /api/testnet/peers/persistent?blockchain_name=planq
```
Output :
```
{"persistent":"3eb12284b7fb707490b8adfda6fa7d94e2fa5cd9@p2p.tendermint.roomit.xyz:16603"}
```

#### Snapshot

* For Get Our Seeds Peers
```
GET - /api/mainnet/snapshot/fileinfo?blockchain_name=planq
GET - /api/testnet/snapshot/fileinfo?blockchain_name=planq
```
Output :
```
{"filename":"2024-07-10-planq-9773721.tar.lzma","size":465960750}
```


#### IBC

* For Get IBC Support
```
GET - /api/mainnet/ibc
```
Output :
```
["planq_7070-2","gitopia","source-1","sentinelhub-2","sgenet-1","osmosis-1"]
```





