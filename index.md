# 0x_nodes

protocol for cross-chain liquidity strategies

## protocol information

### core concepts
- Kernel
- BIOS (mainnet token)
- PFA (staking assets)
- BIOS rewards
- ETH rewards
- Integration
- Strategy
- native chain
- remote chain
- interconnects

## functionality

`kernel.deposit`

`kernel.withdraw`

`userPositions.enterStrategy`

`userPositions.existStrategy`

`kernel.claimEthRewards`

`kernel.claimBiosRewards`

`strategyMap` ...

### Deprecated:
`kernel.claimAllRewards`
`kernel.withdrawAllAndClaim`

## interconnects

0x_nodes interconnect allows deploying assets on one chain and operating them, with yield, on another. This is different than bridging, where assets are permanently moved to the another chain. Interconnects do not move assets across chain. Assets are matched with a liquidity provider on the remote chain. When the rmeote chain releases the assets, they are unlocked on the native chain.

## cross-chain strategies

0x_nodes crosschain strategies are the practice of 

## community strategies

0x_nodes platform is open sourced and ready for community users to publish new strategies. Our governance framework allows nomination and voting of new protocol integrations and strategy availability. 

## how to use system11

0x_nodes system11 is the dapp provided to allow users a GUI for using the protocol. All of the methods listed in "functionality" are present in the dapp.