# Raiden Zero Network (final-hack)

## About this Project

https://github.com/mingderwang/RaidenZero

- A challenge to the Gitcoin Virtual Hackathon: Protect Privacy

[https://gitcoin.co/issue/raiden-network/hackathons/8/4449](https://gitcoin.co/issue/raiden-network/hackathons/8/4449)

- Topic: Use Existing Tools To Increase Privacy In Raiden.


## Goal:
    Our goal is to improve the privacy for the Raiden Network using Zero-knowledge Proof technology.


![](https://paper-attachments.dropbox.com/s_F7C76F48A4C46BF69F90AF8607D867D77A6B659799CFB3F6673D4B1E66BD1394_1594090615810_+2020-07-07+10.56.31.png)



## Team Members
    [@lychees](https://gitcoin.co/profile/lychees) (Xiao Dao) **•** 
    [@mingderwang](https://gitcoin.co/mingderwang) (Ming) **•** 


## Introduction

Even though the current Raiden Network is a off-chain payment system for the Ethereum. The detail transactions off-chain may not unveil on the Ethereum block-chain, but the final settlement still exposes the final amount of the total transactions for both parties on the channel. Therefore, with the metadata on the block-chain, you may also expose your provacy.

So we decided to use [AZTEC](https://github.com/AztecProtocol/AZTEC/tree/7eed9ba3f59b7b8641fcc97c77f03dcdeac37151/packages/protocol)'s Zero-knowledge Proof protocol to cover up the real final amount of transfer tokens when closing the channel. We used two basic proofs in this *experimental process. One is the MintProof, to proof how many (normal ERC20) tokens had been deposited to the channel when the channel is opened.(Actually, MintProof can also continuously adding or removing tokens on the same mintNotes* for the future deposits or withdraws on the same channel.) But a normal ERC20 token does not support* [**confidentialMint**](https://github.com/RaidenZeroNetwork/zkasset/blob/master/PROTOCOL/packages/protocol/contracts/ERC1724/base/ZkAssetMintableBase.sol) for the proof, we need to use [**ZkAssetMintable**](https://github.com/RaidenZeroNetwork/zkasset/blob/master/PROTOCOL/packages/protocol/contracts/ERC1724/ZkAssetMintable.sol) **(**[**ERC1724**](https://github.com/ethereum/EIPs/issues/1724)**)** contract to the demo, That is to mint the same amount of ERC20 tokens as a ZkAsset.

The other proof is called JoinSplitProof, which is used to proof the final result of the settlement when the channel is closed. For example, if the original deposit on the channel is 100, and eventually pay 75 to the target address, we need to have a proof that total mint proof (100) is split into 75 and 25. (We can use the **confidentialTransfer** method of **zkAssetMintable** to have a confidential transfer without disclosing the amount of the transfer.

We does not really implement the whole idea in this project yet. We just wrapped it up with [raiden-cli](https://github.com/raiden-network/light-client/tree/master/raiden-cli) and the [raiden-ts](https://github.com/raiden-network/light-client/tree/master/raiden-ts) (a Raiden Network JavaScript/Typescript lib) to proof of the concept. During this hackathon, we did learn a lot about how to use [Raiden network](https://github.com/raiden-network/raiden), how to deploy the [raiden-contracts](https://github.com/raiden-network/raiden-contracts), how to run a [light-client](https://lightclient.raiden.network/), and even how to write a client or dapp with the raiden-ts library. In the demo video, We just present the concept of adding zero-kownledge proofs feature into the raiden-cli to demonstrate the possible way for a better privacy over the Raiden Network.

In order to allow ERC20 tokens can do both confidentialMint and confidentialTransfer, we need bind this token to a ZkAsset as exchange. In the future, we can also implement an ACP (Asset Control Pool) as a shielded pool for mixer and to call the ZkAsset (ZkERC20) transfer and proof easier.

**Remarks:** * Note is a terminology in AZTEC protocol.


## Diagram 

This is an normal Raiden Network payment workflow;

![](https://paper-attachments.dropbox.com/s_F7C76F48A4C46BF69F90AF8607D867D77A6B659799CFB3F6673D4B1E66BD1394_1594089103616_+2020-07-07+10.29.01.png)


We planned to implement the concept of our “Raiden **Zero** Network” payment system as follow; (We complete the demo for MintProof and JoinSplitProof, other features still WIP.)


![](https://paper-attachments.dropbox.com/s_F7C76F48A4C46BF69F90AF8607D867D77A6B659799CFB3F6673D4B1E66BD1394_1594089296275_+2020-07-07+10.34.41.png)



## What We Have Done (so far)
- To deploy [AZTEC](https://github.com/AztecProtocol/AZTEC)’s (zero-knowledge proof sdk) protocol contracts to Goerli testnet.
- To re-deploy [raiden-contracts](https://github.com/raiden-network/raiden-contracts) to Goerli testnet.
- To modify https://github.com/weilbith/light-client.git for demo raiden-cli payment API.
- To re-build [raiden-ts](https://github.com/raiden-network/light-client/tree/master/raiden-ts) library for raiden-cli.
- To re-build [raiden-cli](https://github.com/raiden-network/light-client/tree/master/raiden-cli) with changes.
- Demo how to run our customized raiden-cli with zk proofs ONLY! (**the original actions of Raiden Network are remain the same**.)


* Remark: We only implemented and demo the SPEC 2 features.

![](https://paper-attachments.dropbox.com/s_F7C76F48A4C46BF69F90AF8607D867D77A6B659799CFB3F6673D4B1E66BD1394_1594089777852_+2020-07-07+10.42.29.png)




## Our Github
https://github.com/RaidenZeroNetwork



## Demo (live video demo → https://youtu.be/CRgwV42l8Eo)

(repo: git@github.com:RaidenZeroNetwork/light-client.git)

    git clone git@github.com:RaidenZeroNetwork/light-client.gi
    cd light-client/raiden-cli
    pnpm install

Step 1. to start a raiden-cli on port 5001 (a RESTful API raiden server)

    $ cd light-client/raiden-cli
    $ node build/index.js -k '/tmp/UTC--2020-07-07T13-56-16.648039000Z--e8b21a66d89401254045bab95b474b52b6fac351' -e 'https://goerli.infura.io/v3/0413092603fe4115b13fb51fdae410a0' --password '' --port 5001

Step 2. to open a new channel.

    $ curl -i -X PUT http://localhost:5001/api/v1/channels -H 'Content-Type: application/json' --data-raw '{"partner_address": "0xeB64730c0E9fEc17816dCCc10F014B2868B7e5fc", "reveal_timeout": "50", "settle_timeout": "500", "token_address": "0x76b335763f779979D7322dB2B73088b8D6001b75", "total_deposit": "1000"}'

Result:

![](https://paper-attachments.dropbox.com/s_F7C76F48A4C46BF69F90AF8607D867D77A6B659799CFB3F6673D4B1E66BD1394_1594145426114_+2020-07-08+2.03.13.png)


Step 3. to issue a channel deposit with curl

    $  curl -i -X PATCH http://localhost:5001/api/v1/channels/0x76b335763f779979D7322dB2B73088b8D6001b75/0x1F916ab5cf1B30B22f24Ebf435f53Ee665344Acf -H 'Content-Type: application/json' --data-raw '{"total_deposit": "10000000"}'

Result: We got a MintProof receipt, which is a confidentialMint, as bellow;

    {
      nonce: 105,
      gasPrice: BigNumber { _hex: '0x3b9aca00', _isBigNumber: true },
      gasLimit: BigNumber { _hex: '0x014c08', _isBigNumber: true },
      to: '0x69416237250eEB33C3a2F764ca5FD63eF478e77A',
      value: BigNumber { _hex: '0x00', _isBigNumber: true },
      data: '0x1b50a0740000000000000000000000000000000000000000000000000000000000010201000000000000000000000000000000000000000000000000000000000000004000000000000000000000000000000000000000000000000000000000000004a203f61f261405ee7198c2f34a27cad12a4fab6a6574379bc58316d214ae52fa2c00000000000000000000000000000000000000000000000000000000000000c00000000000000000000000000000000000000000000000000000000000000320000000000000000000000000000000000000000000000000000000000000036000000000000000000000000000000000000000000000000000000000000003c0000000000000000000000000000000000000000000000000000000000000000306eb8b4f50478922a7162987a9452a423e348d5846a56beb6aba7234d4470fbd138dda5f1e1357be6c55b7fc65b31c45fbb891c7d09bc233fc82191775c384f20100625bc6ee7aff19ad0a7b8bedb4c8fe14589037332f6a769589cdadeecf520dec5f571e3ae326b7cb7855ee7ca3ede21131d42064dd0ee4384b035b9319de04e20a78361281545f7501380e143ba9243b0943785cfdd7981075b5668f6f0313a584b3c0a8486a40658a973370d062a91900d80bb0dda24ea022913c9586c52b6ef6d0ae25d595f39abc62989f09c9d9afe444d854cd86b47bb5024d9b22901325be3ae5bd3c598805dde3b4be12a7cd88e736ea2a2dc376480aec52264ec906cae2073d31d9af85e23d71a82bf101ecde2ded82537d214e0a022b69d690f601c81a207cd1c40ac10877d05c37f42d94851793050213990f2720dcb5d9bf1000164b60d0fa1eab5d56d9653aed9dc7f7473acbe61df67134c705638441c4b92bb1b9b55ffdcf2d7254dfb9be2cb4e908611b4adeb4b838f0442fce79416cf0000000000000000000000000000000000000000000000000000000000000000024f64ca563835ff3a0837d32b185c914df33d2e2f1dba6675c8c8d9d886d05fd164fedfd8bb0803ee703e09bb7e30e827e7fee64f043b484a4ba903bdd95a10b1b97aa665ecdf19b3fa7b8e1359e437f66bb93f99daf88fc728668202d56cd312c3881f5871007cbad270d38ef8bc0771200836714ec28afb5d1c7cf9797ff851d6739c67c7bdb2aaf28d95ddac51eb5169cef2b3cae29f5684802541c2b816b0000000000000000000000000000000000000000000000000000000000000001000000000000000000000000a4c47c176a5e9f62e1ca73881bfaf5428dabb47700000000000000000000000000000000000000000000000000000000000000020000000000000000000000000000000000000000000000000000000000000000000000000000000000000000a4c47c176a5e9f62e1ca73881bfaf5428dabb47700000000000000000000000000000000000000000000000000000000000000e20000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000c100000000000000000000000000000000000000000000000000000000000000216db85524324ac61eee9d579ec2ca397aba0dfee84b89551a8b25f3540e767c6e0100000000000000000000000000000000000000000000000000000000000000213ef1566e252dbb69e720045aa27fe7f0724cacbb5280e833c2b1adbb98d3b7a700000000000000000000000000000000000000000000000000000000000000',
      chainId: 5,
      v: 45,
      r: '0xe6e51313e601f27c695c1d39a0f707c8df12f982cd0f21f1e96f777f57713588',
      s: '0x66f039ec130cdae363dbe30b92c4ec1638e7caa72deb53016ec49bc5bd92cff9',
      from: '0xE8B21A66d89401254045bAb95B474B52B6faC351',
      hash: '0xd71c2ae40a310d6fd87b7b03850e922690a84fe2f90c59b92f0a0262aacfbbe4',
      wait: [Function (anonymous)]
    } <-- the receipt of a confidentialMint
    completed mint proof
    User A successfully deposited  100

Step 4. to close a channel

    curl -i -X PATCH http://localhost:5001/api/v1/channels/0x76b335763f779979D7322dB2B73088b8D6001b75/0x1F916ab5cf1B30B22f24Ebf435f53Ee665344Acf -H 'Content-Type: application/json' --data-raw '{"state": "closed"}'

Result: We got a receipt of a JoinSplitProof (which is a confidentialTransfer), as bellow;

    {
      nonce: 106,
      gasPrice: BigNumber { _hex: '0x3b9aca00', _isBigNumber: true },
      gasLimit: BigNumber { _hex: '0x014c08', _isBigNumber: true },
      to: '0x69416237250eEB33C3a2F764ca5FD63eF478e77A',
      value: BigNumber { _hex: '0x00', _isBigNumber: true },
      data: '0x969884980000000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000056000000000000000000000000000000000000000000000000000000000000004e2000000000000000000000000000000000000000000000000000000000000000105400208ac84c15c173d25e4a1bd9b1c4988b8f71a866303e3c8496c27c89dfb000000000000000000000000e8b21a66d89401254045bab95b474b52b6fac3510000000000000000000000000000000000000000000000000000000000000100000000000000000000000000000000000000000000000000000000000000036000000000000000000000000000000000000000000000000000000000000003a0000000000000000000000000000000000000000000000000000000000000040000000000000000000000000000000000000000000000000000000000000000030056c342d3c011112e8a4d62ec6aa0b462decb2986a5281b10a4efc5f06d10ba11105c721ac80ade7305dae73bdd3685afbef2ea56573ad860fad77bef45105b164fedfd8bb0803ee703e09bb7e30e827e7fee64f043b484a4ba903bdd95a10b1b97aa665ecdf19b3fa7b8e1359e437f66bb93f99daf88fc728668202d56cd312c3881f5871007cbad270d38ef8bc0771200836714ec28afb5d1c7cf9797ff851d6739c67c7bdb2aaf28d95ddac51eb5169cef2b3cae29f5684802541c2b816b1c6d510d01bba765e2bbaaf62dcd1579aded7c85219adc3d86bcf690154a987a038d3ea03c782a174da7f9a09d722a2e210acc94fafcf33f1eed41d7dd96c34c15cb43b659da09a82614d4f807f64a8286a4740ee41f162927278acf46e3f06d1517a473ecc84298f5589ef24e72126ccac279741ad032375a6cec6022701e71172d3cd69837b6304682882240892974f8b2c60419b1d1e6845ed67b1bf60fc3123450711d3061334de3dfab4b3e9462b6b49be7a391be8b3bd16ae8e14f429b000000000000000000000000000000000000000000000000000000000000000025072864fe1e31a39ff22b3dec536748b2a7bcd7dc543167625b235aab29f3a8154b7838dc11d4fe298c2f9fed9b5dbb46602acb2ffb641b15f70b22730846390cc058653d607599e094b7d11951846b9f0af2eca7adb33dd925635ff44f54e813751122e191a0c4b49c40ebc582dc78d2d628f7febc45d348a437c8ca9f533b236288b1d0680b0e810a8b6e250131340d3e70ffc04d7001e643ea0425ed006c0000000000000000000000000000000000000000000000000000000000000001000000000000000000000000a4c47c176a5e9f62e1ca73881bfaf5428dabb47700000000000000000000000000000000000000000000000000000000000000020000000000000000000000004e2ccf6081d20209c8ad3a8b259c563626186172000000000000000000000000a4c47c176a5e9f62e1ca73881bfaf5428dabb47700000000000000000000000000000000000000000000000000000000000000e20000000000000000000000000000000000000000000000000000000000000002000000000000000000000000000000000000000000000000000000000000008000000000000000000000000000000000000000000000000000000000000000c1000000000000000000000000000000000000000000000000000000000000002161b9c7c19f06505647ac91ef5a816c48aa72395a94669892c28dc4db1f5b678d0000000000000000000000000000000000000000000000000000000000000000211ee875bcf15a18b22486bd32caac6e24225e1b1f56d8e955b4f1b3a8ce839fed0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000417d027501cb485f5c24f8a3c01ae3e7e30050cb445e4cc722a614ede897db7948230c23e4f4c2b47edd6ed20b0b51c237bd64b5c2a515df5456f859f0348b8a9f1b00000000000000000000000000000000000000000000000000000000000000',
      chainId: 5,
      v: 45,
      r: '0xc0357a22f27ad6b02e86aa710bd3bc40118a8754b18321220c3970290dfc1c5d',
      s: '0x64baabf28bb313468c59a2bef3e1ea3939ee6138d07bde41c75251e0fb4bc079',
      from: '0xE8B21A66d89401254045bAb95B474B52B6faC351',
      hash: '0xf763c9d04c4c049448d309c1a0b21101c1fb36c530a0fdd3ee0fcac1361d5d52',
      wait: [Function (anonymous)]
    } <-- the receipt of a confidentialTransfer


## Future (Open Source) Projects
- To implement an Access Control Pool, ACP (as a RESTful API or GraphQL server) to work with ZkAsset (ZkERC20 Tokens) for easy proofs and exchanges with ERC20 tokens.
- To do more customization on raiden-ts and raiden-contracts for the Raiden Zero Network (RZN).
- To write a raiden dapp to do the RZN secure payment demo.
- To implement RZN as a Raiden Network service for anyone to utilize the confidential payment channels with the Zero-Knowledge Proof technology easier.
- etc…

