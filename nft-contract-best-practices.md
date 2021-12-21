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

### Consider approving OpenSea trades to save sellers gas

Before you can sell an NFT on OpenSea, you have to approve OpenSea to trade on your behalf. This happens for each contract through `setApprovalForAll`.

You can bypass this step for your users by writing your contract in a way that automatically considers OpenSea approved to trade on their behalf.

https://twitter.com/mannynotfound/status/1470535477191221261
https://github.com/divergencetech/ethier/blob/main/contracts/erc721/OpenSeaGasFreeListing.sol

### Considerations for generating pseudo-random numbers

- `block.timestamp` can be modified by miners within a range of time. Don't use only this as a source of randomness.
- TODO: something about block difficulty going away with PoS
