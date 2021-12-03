# 0x_nodes

protocol for cross-chain liquidity strategies

## protocol information

### core concepts
- `Kernel`
    The kernel is the main tokenholder and primary entry point for system configuration on each chain.
- `BIOS (mainnet token)`
    $BIOS is the utility token for 0xnodes. It's used for staking, liquidity, bonding, as well as a native and remote settlement layer asset (NSLA and RSLA respectively).
- `PFA (Protocol Fee Accruals by staking assets)`
    Staked assets in a kernel that are not deployed to a strategy will recieve PFA. Staked assets are not leveraged or deployed. PFA is funded by a tax on yield repatriation. 
- `BIOS rewards`
    BIOS is awarded to the PFA as part of the epoch rewards.
- `ETH rewards`
    ETH rewards are the NSLA for eth mainnet. 
- `Integration`
    The platform can deploy into integrations according to user strategy positions. Any yield-bearing contract can be integrated.
- `Strategy`
    A strategy is an array of integrations. When a user deposits into the strategy, the integrations are funded and can be deployed.
- `Deploy`
    Yield Manager and Integration deploys take the pooled user assets and move them according to the configuration. 
- `Native chain`
    The currently connected chain.
- `Remote chain`
    Any chain that is not the currently connected chain. 
- `Interconnects`
    0xnodes transport layer for interaction between chains. Interconnects can transport synthetics, LP positions, relay data, and other assets.
## functionality
- `kernel.deposit`
    Send assets to the kernel to begin participation in the protocol. Enter into the 0x_nodes system by depositing assets into the Kernel

- `kernel.withdraw`
    Remove assets from the kernel.

- `userPositions.enterStrategy`
    Put your assets to work: earn yield on your assets by staking them on the yield strategies of your choice

- `userPositions.exitStrategy`
    Unwind from a strategy so that your assets are available to withdraw or to enter other strategies with

- `kernel.claimEthRewards`
    Withdraw all ether rewards you have earned in the system

- `kernel.claimBiosRewards`
    Withdraw all BIOS rewards you have earned in the system

### Deprecated:
`kernel.claimAllRewards`
`kernel.withdrawAllAndClaim`
Deprecated because of gas costs.

## Strategies

### SAAL: Sushi Aquire And Liquidate
SAAL strategies participate in sets of 3farms. 3farms are pools of three sushi farms (from masterchef v1 or v2). SAL strategies deploy into the 3farm evenly and execute a double-stack (V2) deployment, where the two-token deployment is then staked for sushi and rewards. Sushi and rewards yield is immediately liquidated and make available to users, PFA, and kernel. Users recieve liquidated yield in native chain tokens (ETH, MATIC, etc).

## interconnects

Interconnects allow deploying assets on one chain and operating them, with yield, on another. This is different than bridging, where assets are permanently moved to the another chain. Interconnects do not move assets across chain. Assets are matched with a liquidity provider on the remote chain and can participate in 0xnodes strategies on the remote chain. When the remote chain releases the assets, they are unlocked on the native chain.

## cross-chain strategies

0x_nodes crosschain strategies allow you to use your assets located on one chain to participate in yield earning opportunities located on a different chain. Through the 0x_nodes interconnects protocol you can access cross-chain yield painlessly and efficiently.

## community strategies

0x_nodes platform is open sourced and ready for community users to publish new strategies. Our upcoming governance framework will allow nomination and voting of new protocol integrations and strategy availability. See [AMM integration instructions](amm_integrations)

## how to use system11

0x_nodes [system11](https://system11.0xnodes.io) is the dapp provided to allow users a GUI for using the protocol. All of the methods listed in "functionality" are present in the dapp.
