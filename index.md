# 0x_nodes

protocol for cross-chain liquidity strategies

## protocol information

### core concepts

- Kernel - the kernel is the core to the entire system. each network supported by the 0x_nodes system has its own optimized kernel which you use to deposit or withdraw assets

- BIOS (mainnet token) - $BIOS is the native token of 0x_nodes system, an ERC-20 standard token on the Ethereum mainnet. $BIOS can be deposited into the system to enhance the yield you receive but is not required

- PFA (staking assets)

- BIOS rewards - you can accrue rewards in $BIOS that can be used to increase your yield from the 0x_nodes system

- ETH rewards - you can accrue rewards in ether

- Integration - 0x_nodes has built numerous integrations such that from within the system you can interact with numerous popular defi protocols in a consistent manner

- Strategy - The 0x_nodes community contributes and votes on yield earning strategies to run in the system all built from our universal integration interfaces. You choose which strategies you want to participate in.

- native chain - The network you are connected to the 0x_nodes system through. Assets are moved within the native chain into the 0x_nodes system.

- remote chain - Other networks are remote chains. The 0x_nodes system allows you to leverage your liquidity across our interconnects protocol to earn yield in a remote chain through a synthetic position.

- interconnects - The 0x_nodes kernels deployed to different chains will all interact via our interconnects protocol. When launched, this will allow you to use your assets from one chain to participate in yield earning strategies on other remote chains.

## functionality

`kernel.deposit` - enter into the 0x_nodes system by depositing assets into the Kernel

`kernel.withdraw` - pull your assets back out of the system

`userPositions.enterStrategy` - put your assets to work: earn yield on your assets by staking them on the yield strategies of your choice

`userPositions.exitStrategy` - unwind from a strategy so that your assets are available to withdraw or to enter other strategies with

`kernel.claimEthRewards` - withdraw all ether rewards you have earned in the system

`kernel.claimBiosRewards` - withdraw all BIOS rewards you have earned in the system

`kernel.claimAllRewards` - withdraw all rewards you have earned in the system

<!-- `strategyMap` ... -->

### Deprecated:

`kernel.claimAllRewards`
`kernel.withdrawAllAndClaim`

## interconnects

0x_nodes interconnect allows deploying assets on one chain and operating them, with yield, on another. This is different than bridging, where assets are permanently moved to the another chain. Interconnects do not move assets across chain. Assets are matched with a liquidity provider on the remote chain. When the remote chain releases the assets, they are unlocked on the native chain.

## cross-chain strategies

0x_nodes crosschain strategies allow you to use your assets located on one chain to participate in yield earning opportunities located on a different chain. Through the 0x_nodes interconnects protocol you can access cross-chain yield painlessly and efficiently.

## community strategies

0x_nodes platform is open sourced and ready for community users to publish new strategies. Our governance framework allows nomination and voting of new protocol integrations and strategy availability.

## how to use system11

0x_nodes system11 is the dapp provided to allow users a GUI for using the protocol. All of the methods listed in "functionality" are present in the dapp.
