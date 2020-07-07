# Raiden Zero Network (final-hack)

## About this Project

https://github.com/mingderwang/RaidenZero

- A challenge for the Gitcoin Virtual Hackathon (Protect Privacy)

[https://gitcoin.co/issue/raiden-network/hackathons/8/4449](https://gitcoin.co/issue/raiden-network/hackathons/8/4449)


- **Our Goal is to improve the privacy for the Raiden Network using Zero-knowledge Proofs.**


![](https://paper-attachments.dropbox.com/s_F7C76F48A4C46BF69F90AF8607D867D77A6B659799CFB3F6673D4B1E66BD1394_1594090615810_+2020-07-07+10.56.31.png)



## Team Members
    [@lychees](https://gitcoin.co/profile/lychees) (Xiao Dao) **•** 
    [@mingderwang](https://gitcoin.co/mingderwang) (Ming) **•** 


## Introduction
    Eventhough the current Raiden Network is a off-chain payment system for the Ethereum. The detail transactions off-chain may not unveal on the Ethereum block-chain, but the final settlement still exposes the final result of the total transactions for both parties on the channel.
    
    So we deside to use AZTEC's Zero-knowledge Proof technology to cover up the real final balance when closing the channel. We use two basic proofs in this experimental process. One is the MintProof, to proof how many (normal ERC20) tokens had been deposited to the channel when the channel is opened.(Actually, MintProof can also continuously adding or removing tokens on the same mintNotes for the futher deposits or withdraws on the same channel.) But a normal ERC20 token does not support confidentialMint for the proof.
    
    The other proof is called JoinSplitProof, which is used to proof the final result of the settlement when the channel is closed. For example, if the original deposit on the channel is 100, and eventually pay 75 to the target address at the end, we need to have a proof that total mintnotes (100) is splited to 75 and 25. (We can use confidentialTransfer method to do the transfer without disclosing the amount of the transfer.
    
    We are not really implement the whole idea in this project yet, we just wrap it up with raiden-cli and the raiden-ts (raiden network JS lib) to proof the concept.
    
    During this hackathon, we do learn a lot about how to use raiden network, how to deploy the raiden-contracts, how to run a light-client, and even how to write a client or dapp with the raiden-ts library. In the demo video, We just show the concept of adding zero-kownledge proofs feature into the raiden-cli to demostrate the possible way for a better privacy over the Raiden Network.
    
    In order to allow ERC20 tokens can do both confidentialMint and confidentialTransfer, we need bind this token to a ZkAsset as exchange. In the future, we can also implement an ACP (Asset Control Pool) as a shielded pool for mixer and to call the ZkAsset (ZkERC20) transfer and proof easier.


## Diagram 

This is an normal Raiden Network payment workflow;

![](https://paper-attachments.dropbox.com/s_F7C76F48A4C46BF69F90AF8607D867D77A6B659799CFB3F6673D4B1E66BD1394_1594089103616_+2020-07-07+10.29.01.png)


We planned to implement the concept of Raiden Zero Network payment system as follow;


![](https://paper-attachments.dropbox.com/s_F7C76F48A4C46BF69F90AF8607D867D77A6B659799CFB3F6673D4B1E66BD1394_1594089296275_+2020-07-07+10.34.41.png)



## What We Have Done (so far)
- To deploy AZTEC’s (zero-knowledge proof sdk) contracts to Goerli testnet.
- To re-deploy raiden-contracts to Goerli testnet.
- To test https://github.com/weilbith/light-client.git for payments API.
- To build raiden-ts library with modification.
- To build raiden-cli with modification.
- Demo how to run our customized raiden-cli with a payment (includes ZkAsset proofs)


![](https://paper-attachments.dropbox.com/s_F7C76F48A4C46BF69F90AF8607D867D77A6B659799CFB3F6673D4B1E66BD1394_1594089777852_+2020-07-07+10.42.29.png)



## Future (Open Source) Projects
- To implement the ACP (a RESTful API or GraphQL server) to work with ZkAsset (Tokens) proof and exchange.
- To do more customization on raiden-ts and raiden-contracts for the Raiden Zero Network (RZN).
- To write a raiden dapp to do the RZN payment demo.
- To implement RZN as a RN service for anyone can utilize the private payment channels.


