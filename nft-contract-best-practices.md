# NFT contract best practices

I've come across a handful of these and can no longer keep them in my head, so I'm jotting them down here for future reference. Feel free to submit a PR to add to this list!

### Consider gas savings by not using `ERC721Enumerable`

TODO

### Consider protecting against minting by smart contracts

If you want to avoid one smart contract creating many other smart contracts to get past wallet minting limits, consider comparing the message sender against the transaction origin:
```solidity
require(tx.origin == msg.sender);
```
The `tx.origin` is the address that kicked-off the transaction and the `msg.sender` is the address that called your contract. These are often the same, but can be different when another contract is acting on behalf of the original sender.

This can still be worked around with a script that kicks off a bunch of transaction, but adds a little bit of friction to malicious actors.

Source: https://twitter.com/dievardump/status/1435322083060424704
