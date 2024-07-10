---
description: Data structure of Blockchain
---

# Block, Chain and Consensus

## Hash

**Explanation**: The principle of hashing is to convert input of any length into output of fixed length through the hash algorithm.

**Characteristic**: the same input will definitely get the same output, therefore different inputs will most likely get different outputs.

**Example**: use <mark style="color:red;">`md5sum`</mark> in the shell command line to calculate the MD5 hash of any charater.

```sh
$ md5sum <<< haha
7494ab07987ba112bd5c4f9857ccfb3f  -
$ md5sum <<< hehe
e4439267203fb5277d347e6cd6e440b5  -
$ md5sum <<< hhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhh
6f2362c812dcfd693da2e3ae537cfb4
```

**Hash algorithms**:&#x20;

* File tamper-proof: MD5
* Bitcoin mining: SHA256
* Data fragment proof: Merkle root
* Text deduplication: SimHash

## Block

A block is composed of a block header and a transaction list (tx list). Blocks are linked together into a chain structure through the hash of the block header.

### Block header

Bitcoin's block header&#x20;

```cpp
int32_t nVersion; 
uint256 hashPrevBlock; 
uint256 hashMerkleRoot; 
uint32_t nTime; 
uint32_t nBits; 
uint32_t nNonce;
```

Ethereum's block header

```solidity
ParentHash  common.Hash    `json:"parentHash"       gencodec:"required"`
UncleHash   common.Hash    `json:"sha3Uncles"       gencodec:"required"`
Coinbase    common.Address `json:"miner"            gencodec:"required"`
Root        common.Hash    `json:"stateRoot"        gencodec:"required"`
TxHash      common.Hash    `json:"transactionsRoot" gencodec:"required"`
ReceiptHash common.Hash    `json:"receiptsRoot"     gencodec:"required"`
Bloom       Bloom          `json:"logsBloom"        gencodec:"required"`
Difficulty  *big.Int       `json:"difficulty"       gencodec:"required"`
Number      *big.Int       `json:"number"           gencodec:"required"`
GasLimit    uint64         `json:"gasLimit"         gencodec:"required"`
GasUsed     uint64         `json:"gasUsed"          gencodec:"required"`
Time        uint64         `json:"timestamp"        gencodec:"required"`
Extra       []byte         `json:"extraData"        gencodec:"required"`
MixDigest   common.Hash    `json:"mixHash"`
Nonce       BlockNonce     `json:"nonce"`


// BaseFee was added by EIP-1559 and is ignored in legacy headers.
BaseFee *big.Int `json:"baseFeePerGas" rlp:"optional"`
```

Currently, we only focus on two of the fields:

1. <mark style="color:red;">`hashPrevBlock`</mark> /<mark style="color:red;">`ParentHash`</mark>，hash of previous block header
2. <mark style="color:red;">`hashMerkleRoot`</mark>/<mark style="color:red;">`TxHash`</mark>，hash of tx list

### Block body

<figure><img src="../.gitbook/assets/block-body.png" alt=""><figcaption><p>The block body is the tx list</p></figcaption></figure>

1. The block body is the tx list, and the block header points to the unique tx list through <mark style="color:red;">`TxHash`</mark>.
2. The hash algorithm used to obtain <mark style="color:red;">`TxHash`</mark> from the tx hash list is called a Merkle tree, which has special properties compared to other hash algorithms (it can succinctly prove the existence of a tx within it).

## Chain

<figure><img src="../.gitbook/assets/chain.png" alt=""><figcaption><p>Form of a chain</p></figcaption></figure>

1. The block header points to the unique previous block header through the <mark style="color:red;">`ParentHash`</mark>, and eventually forms a chain.
   1.  A block with a height of 0 is called Genesis, or the Genesis Block.

       1. Hardcoded in the program ([Github](https://github.com/bitcoin/bitcoin/blob/v22.0/src/chainparams.cpp#L53))
       2. The Genesis Block of Bitcoin can be found at [btc.com](https://btc.com/btc/block/0), along with the message left by Satoshi Nakamoto.



       <figure><img src="../.gitbook/assets/EqyhCKuXIAAOl0z.jpeg" alt=""><figcaption><p>The headline of the newspaper article recorded on the Bitcoin genesis block</p></figcaption></figure>
   2. The block with the highest height is called the Tip, which is the latest block
   3. You can observe the status of the Etherum chain on [Etherscan](https://etherscan.io/blocks)
2. The combination of <mark style="color:red;">`TxHash`</mark> and <mark style="color:red;">`ParentHash`</mark> gives the blockchain its immutable characteristics.

## Consensus

Only the knowledge that affects data structures is discussed, without delving into consensus algorithms.

### Block reward

1.  **Why**: Incentivize nodes to produce blocks

    * Agree on the effectiveness of tx

    <figure><img src="../.gitbook/assets/ds.png" alt=""><figcaption><p>Double-spending attack of Bitcoin</p></figcaption></figure>

* Agree on the sequence of tx

2. **How**: <mark style="color:red;">`Coinbase`</mark>&#x20;
   1.
