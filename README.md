# Abstract

**Sesscion-Key Aspect** allows EoA to extend several sub-keys named **Session Key**.

Session keys are able to **stand in for** EoA private key to sign a **specific transaction**. These keys will automatically expire at the block height, which is set by EoA. They are also limited to sign specific transactions, calling only specific smart contract methods.

**With Session-Key Aspect, you can enable following features for your dApp:**
* Enable On-Click-Trading for defi protocol
* Improve UX and wallet security for mini web app (like PWA, bot and TWA in Telegram)
* Use your dApp like web2 products: login once, and click without interacting with the wallet

# Project Intro

* Folder [aspect](https://github.com/artela-network/session-key-aspect/blob/main/aspect/README.md) implements the session key Aspect;
* Folder [js_client](https://github.com/artela-network/session-key-aspect/blob/main/js_client/README.md) implements the session key Aspect javascript client.



# Session-key Overview

## 1. EoA binding session key to Aspect

![截屏2023-11-09 15.37.26.png](https://github.com/artela-network/session-key-aspect/blob/main/img/2023-11-09-15.37.26.png)



Additional info:

- `Specific contract` is a contract address. For example, if it’s a DEX contract address, it means that the session key is only limited to calling an Artex contract.

- `Specific methods` is a list of method signature of `Specific contract`. For example, if it’s `[0x0000CAFE, 0xCAFE0000,]` , it means that the session key is only limited to this two method.

  

## 2. Use session key to sign transaction

![截屏2023-11-09 15.32.38.png](https://github.com/artela-network/session-key-aspect/blob/main/img/2023-11-09-15.32.38.png)

Additional info :

- The `from` is still the address of EoA.

- The signature `v,r,s` is generated by private key of sKey.

  

## 3. Artela verify the transaction

![截屏2023-11-09 15.52.20.png](https://github.com/artela-network/session-key-aspect/blob/main/img/2023-11-09-15.52.20.png)

Additional info

- The transaction may be signed by the EoA privaten point needs to be verified by the EoA public key again if the Aspect returns key, so the joi false.

  

## 4. Call contract

![截屏2023-11-09 15.54.18.png](https://github.com/artela-network/session-key-aspect/blob/main/img/2023-11-09-15.54.18.png)

Additional info

- Smart contract doesn’t know which keys sign the tx.

- When the smart contract accesses `msg.sender` , the value is `from` in the transaction.

  

# Implements

Session-key Aspect project contains three components.

- **Client**, `sessioin-key-aspect.js`, a client for the dApp front end to use session key.
- **Aspect**, wasm bytecode deployed on Artela
- **Explorer,** extend Artela explorer to show Aspect info


![截屏2023-11-09 16.16.17.png](https://github.com/artela-network/session-key-aspect/blob/main/img/2023-11-09-16.16.17.png)



# Client Details

`sessioin-key-aspect.js` is the client of Session-key Aspect.

It contains 3 key modules:

- session-key store
- signer
- aspect-client

**session-key store** offers:

- generate key pair in the front end

- manage key pair in the browser cache

  ( including store, load, clear, etc.)

**signer** offers:

- high-level API for dApp to use the session key

- load session key from

  

# What’s the difference between AA

**It works like AA (abstract account).** For example, we can design a kind of AA that can be signed by a main private key and some session keys.

**But it will be more flexible than AA.**

- EoA can not extend to AA.

  If you want to use session-key enable AA, you have to transfer your assets from your EoA to a new AA. This user experience might be terrible in some situations.

  By Session-Key Aspect, an EoA can send a transaction to bind it with Session-Key Aspect, and then the EoA will upgrade to a kind of AA that supports the session key. It needn’t transfer its assets.

- Users only need to manage one key.

  If you use AA, you might manage several AA wallets with different function. By using Aspect to extend EoA, you just use one wallet with several extension functions.
