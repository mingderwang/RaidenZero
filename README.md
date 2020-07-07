# Raiden Zero Network (final-hack)

## About this Project

https://github.com/mingderwang/RaidenZero

- A challenge to the Gitcoin Virtual Hackathon: Protect Privacy

[https://gitcoin.co/issue/raiden-network/hackathons/8/4449](https://gitcoin.co/issue/raiden-network/hackathons/8/4449)

- Use Existing Tools To Increase Privacy In Raiden.


## Goal:
    Our goal is to improve the privacy for the Raiden Network using Zero-knowledge Proof technology.


![](https://paper-attachments.dropbox.com/s_F7C76F48A4C46BF69F90AF8607D867D77A6B659799CFB3F6673D4B1E66BD1394_1594090615810_+2020-07-07+10.56.31.png)



## Team Members
    [@lychees](https://gitcoin.co/profile/lychees) (Xiao Dao) **•** 
    [@mingderwang](https://gitcoin.co/mingderwang) (Ming) **•** 


## Introduction

Even though the current Raiden Network is a off-chain payment system for the Ethereum. The detail transactions off-chain may not unveil on the Ethereum block-chain, but the final settlement still exposes the final amount of the total transactions for both parties on the channel. Therefore, with the metadata on the block-chain, you may also expose your provacy.

So we decided to use [AZTEC](https://github.com/AztecProtocol/AZTEC/tree/7eed9ba3f59b7b8641fcc97c77f03dcdeac37151/packages/protocol)'s Zero-knowledge Proof protocol to cover up the real final amount of transfer tokens when closing the channel. We used two basic proofs in this *experimental process. One is the MintProof, to proof how many (normal ERC20) tokens had been deposited to the channel when the channel is opened.(Actually, MintProof can also continuously adding or removing tokens on the same mintNotes* for the future deposits or withdraws on the same channel.) But a normal ERC20 token does not support* **confidentialMint** for the proof, we need to use **ZkAssetMintable (**[**ZkERC20**](https://github.com/ethereum/EIPs/issues/1724)**)** contract to the demo.

The other proof is called JoinSplitProof, which is used to proof the final result of the settlement when the channel is closed. For example, if the original deposit on the channel is 100, and eventually pay 75 to the target address, we need to have a proof that total mint proof (100) is split into 75 and 25. (We can use the **confidentialTransfer** method of **zkAssetMintable** to have a confidential transfer without disclosing the amount of the transfer.

Remarks: * Note is a terminology in AZTEC protocol.


    
    We are not really implement the whole idea in this project yet, we just wrap it up with raiden-cli and the raiden-ts (raiden network JS lib) to proof the concept.
    
    During this hackathon, we do learn a lot about how to use raiden network, how to deploy the raiden-contracts, how to run a light-client, and even how to write a client or dapp with the raiden-ts library. In the demo video, We just show the concept of adding zero-kownledge proofs feature into the raiden-cli to demostrate the possible way for a better privacy over the Raiden Network.
    
    In order to allow ERC20 tokens can do both confidentialMint and confidentialTransfer, we need bind this token to a ZkAsset as exchange. In the future, we can also implement an ACP (Asset Control Pool) as a shielded pool for mixer and to call the ZkAsset (ZkERC20) transfer and proof easier.


## Diagram 

This is an normal Raiden Network payment workflow;

![](https://paper-attachments.dropbox.com/s_F7C76F48A4C46BF69F90AF8607D867D77A6B659799CFB3F6673D4B1E66BD1394_1594089103616_+2020-07-07+10.29.01.png)


We planned to implement the concept of our Raiden “**Zero”** Network payment system as follow;


![](https://paper-attachments.dropbox.com/s_F7C76F48A4C46BF69F90AF8607D867D77A6B659799CFB3F6673D4B1E66BD1394_1594089296275_+2020-07-07+10.34.41.png)



## What We Have Done (so far)
- To deploy [AZTEC](https://github.com/AztecProtocol/AZTEC)’s (zero-knowledge proof sdk) contracts to Goerli testnet.
- To re-deploy [raiden-contracts](https://github.com/raiden-network/raiden-contracts) to Goerli testnet.
- To test https://github.com/weilbith/light-client.git (a fork) for payments API.
- To build [raiden-ts](https://github.com/raiden-network/light-client/tree/master/raiden-ts) library with modification.
- To build [raiden-cli](https://github.com/raiden-network/light-client/tree/master/raiden-cli) with modification.
- Demo how to run our customized raiden-cli with a payment (includes ZkAsset proofs)


![](https://paper-attachments.dropbox.com/s_F7C76F48A4C46BF69F90AF8607D867D77A6B659799CFB3F6673D4B1E66BD1394_1594089777852_+2020-07-07+10.42.29.png)



## Future (Open Source) Projects
- To implement an Access Control Pool, ACP (as a RESTful API or GraphQL server) to work with ZkAsset (ZkERC20 Tokens) for easy proofs and exchanges with ERC20 tokens.
- To do more customization on raiden-ts and raiden-contracts for the Raiden Zero Network (RZN).
- To write a raiden dapp to do the RZN secure payment demo.
- To implement RZN as a Raiden Network service for anyone to utilize the private payment channels with the Zero-Knowledge Proof technology.
- etc…


