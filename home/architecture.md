---
description: Architecture RoomIT
---

# Architecture Diagram Platform

<figure><img src="https://server.gitopia.com/raw/roomit-xyz/stake.roomit.xyz/master/img/powered/RoomIT-Network.png" alt="">
<figcaption><p>Architecture RoomIT</p></figcaption></figure>

## Overview

The Roomit architecture is built independently from scratch, utilizing robust hardware components. This architecture is designed to be isolated, secure, and reliable. Its purpose is to ensure the security of blockchain validation, VPN services, and decentralized infrastructure services. Additionally, there are other services such as web service endpoints (API, RPC, GRPC), snapshot services, data providers, and more. All services are designed with redundancy, automation, security, monitored and boast a strong SLA with an average uptime of 99.99%.


## Main Component
The Main Component Roomit provided is following:

**1. Validation Blockchain (Mainnet dan Testnet)**

Roomit has conducted extensive validations across various blockchains. These validations are carried out on both production and staging systems. In the blockchain ecosystem, the production system refers to the mainnet, which is the primary or production network of a blockchain project. This is the final version of the network released for public use and operates with cryptocurrency that holds real value.
A testnet in blockchain is a trial network used for development, testing, and experimentation before features or changes are applied to the main network (mainnet). The details are as follows:

**Mainnet**
- Gitopia Blockchain
- Planq Network
- Source Protocol
- SGE Protocol
- Sentinel Network
- Selfchain 
- Nibiru
- Band Protocol
- Entangle
- Block Network
- Gnosischain
- Dan Lain lain

**Testnet**
- Aptos (AIT2-3)
- Celestia
- Neutron
- OKP4
- Masa Finance
- QGov
- Arkeo
- Taiko
- Alephzero
- Gitopia
- Planq
- SGE
- Namada
- Entangle 
- Selfchain
- Initia
- Aleo
- Dan Lain lain

**2. VPN Decentralized (DVPN)**

A type of virtual private network that is distributed and managed by many nodes or participants in the network, unlike traditional VPNs which are managed by a single central entity, is called a decentralized VPN. We also provide VPN services, which are connected to blockchain. The services we offer are as follows:

- Sentinel
- Mysterium
- NYM

## FLOW
From the architecture diagram above, the following aspects can be concluded:

** Users **
Users can contribute and invest their tokens to decentralize the network or use the VPN services we provide over the internet. Additionally, users can perform transactions or monitor their investments through the web services we offer, such as:

- https://stake.roomit.xyz
- https://explorer.tendermint.roomit.xyz
- https://stake.roomit.xyz/analytics
- https://health.roomit.xyz/status/roomit-mainnet
- https://monittoring.roomit.xyz

** Administrator **
Since the system is well isolated, administrators need VPN access, SSH Public Key, and OTP Access to enter the system. Access is only possible through a single gateway. Roomit is truly designed with a baseline and isolated security system.


** System **
Our blockchain machines only have P2P connection access. The function of this P2P connection is used for the connection between validators to perform transactions.


