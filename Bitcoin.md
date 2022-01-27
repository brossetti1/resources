# [Script](https://en.bitcoin.it/wiki/Script)

Script is:
- stack-based,
- processed from left to right
-  It is intentionally not Turing-complete
-  it has no loops.

A script is essentially a list of instructions recorded with each transaction that describe how the next person wanting to spend the Bitcoins being transferred can gain access to them. The script for a typical Bitcoin transfer to destination Bitcoin address D simply encumbers future spending of the bitcoins with two things: the spender must provide

1) a public key that, when hashed, yields destination address D embedded in the script, and
2) a signature to prove ownership of the private key corresponding to the public key just provided.


## opcodes, commands, or functions

- [Constants](https://en.bitcoin.it/wiki/Script#Constants)
- [Flow Control](https://en.bitcoin.it/wiki/Script#Flow_control)
- [Stack](https://en.bitcoin.it/wiki/Script#Stack)
- [Slice](https://en.bitcoin.it/wiki/Script#Slice)
- [Bitwise Logic](https://en.bitcoin.it/wiki/Script#Bitwise_logic)
- [Arithmetic](https://en.bitcoin.it/wiki/Script#Arithmetic)
- [Crypto](https://en.bitcoin.it/wiki/Script#Crypto)
- [Locktime](https://en.bitcoin.it/wiki/Script#Locktime)
- [Pseudo Words](https://en.bitcoin.it/wiki/Script#Pseudo-words)
- [Reserved Words](https://en.bitcoin.it/wiki/Script#Reserved_words)

## History

Satoshi developed Bitcoin in private before releasing **Bitcoin 0.1**.
Script had several serious bugs when Bitcoin was first released, and some bugs still exist.
It's clear that Satoshi didn't thoroughly test Script before releasing Bitcoin.
Each opcode is pretty much self-contained.

# Two common scripts for Bitcoin

- [Pay to Pubkey Hash](https://en.bitcoinwiki.org/wiki/Pay-to-Pubkey_Hash)
- [Pay to script hash](https://en.bitcoin.it/wiki/Pay_to_script_hash)
