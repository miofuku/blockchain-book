---
description: Data structure of Blockchain
---

# Block, Chain and Consensus

## Hash

**Explanation**: The principle of hashing is to convert input of any length into output of fixed length through the hash algorithm.

**Feature**: the same input will definitely get the same output, therefore different inputs will most likely get different outputs.

**Example**: use `md5sum` in the shell command line to calculate the MD5 hash of any charater.

```sh
$ md5sum <<< haha
7494ab07987ba112bd5c4f9857ccfb3f  -
$ md5sum <<< hehe
e4439267203fb5277d347e6cd6e440b5  -
$ md5sum <<< hhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhh
6f2362c812dcfd693da2e3ae537cfb4
```

**Hash algorithms**:

* File tamper-proof: MD5
* Bitcoin mining: SHA256
* Data fragment proof: Merkle root
* Text deduplication: SimHash
