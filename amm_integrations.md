# 0x_nodes

protocol for cross-chain liquidity strategies

## AMM Integration Instructions

Community-contributed AMM integrations are welcome and can follow these guidelines:

There are now two types of integration:

- Automated market maker - Uniswap, sushiswap, etc
- Non-AMM - Aave, Yearn, etc
- The distinction is that AMM integrations require 2 tokens be deposited at a time.

A Strategy is now defined as:

```
{
	name: string
	integrations: [
		{
			integration: address,
			ammPoolID: uint32 // ID of underlying pool, 0 = single token integration
		}
	],
	tokens: [
		{
			integrationPairIdx:uint256, // Corresponds to integrations array. A token can be used in multiple integrations
			token: address, // AMM pool must support this token, or it reverts.
			weight: uint32 // The % of the whole to allocate to the integration/pool 
				pair. This can be 0 to prevent an integration from receiving any.
		}
	]
}
```

- Normalize the weighting system so every weight is within the same magnitude.
```ie 100% == 100,000 => Each token's total weight should add up to 100,000.
```
 - Error to be thrown when adding invalidly weighted strategy.
    - Weight is arbitrary, as long as it's greater than 100. More zeros means more precision. Deployment should probably use ≥ 1e18.
    - Weight is per token within a strategy.
- Reserves are calculated per token and are taken from the yield as a fixed percentage every harvest.
- Integration-Pair aka IP is the combination of integration address and pool ID.
- Track:
    - User ⇒ strategy ⇒ token ⇒ balance
    - User ⇒ token ⇒ balance
    - Token ⇒ balance (total token balance across all IP)
    - Strategy ⇒ token ⇒ balance (token balance per strategy)
- If a single asset deposit integration is provided with > 1 tokens to deposit, it will process each deposit in order, and in a single request.
    - The system can handle any number of tokens being allocated to a single asset deposit integration.
- For dual asset deposit integrations such as Uniswap, each pool must be added separately to the strategy (ie there could be a strategy with 3 integrations, all of which are uniswap, just different pools).
    - Dual asset deposit integrations can only accept deposits of two token types at a time. An error should be thrown if an admin attempts to create a strategy with more or less than this for a dual asset deposit integration
    - How do we deal with uniswap pool ratios (token pairs must be added in a specific ratio to match the pool, or risk being arbitraged away)?
        - ~~If we remove the current *deposit then enter* flow, and have users enter a strategy directly, we can deploy their funds in the correct ratio, and leave any excess in the kernel for withdrawal or deployment when the ratio changes. The user should be made aware of this before entering a strategy that features this.~~  **We're keeping the deposit → enter flow.**
            - This is undergoing a small adjustment to it's functionality
        - There should be a distinction in the front end, and the relevant contract methods between *deployed funds* and *deposited funds*.

YieldManager to process integration asset deployment thus:or each integration
	for each pool in the integration
		Get pool balance/s (returns (balance A, balance B) where balance B is 0 for single asset deposit integrations)
		Get pool token requirements
		If balance < required then top up
		Else if balance > required then withdraw

- We handle deployment to integrations where the balances may vary over the course of yield generation in the following manner:
    - Examples: Range orders on uniswap, yield farming on any AMM projects
    - We could store amounts to deploy instead of absolute balances. These get reset each deployment. That way we aren't fixing the integration at a hard balance.
        - How does this handle withdrawing due to weight change?
            - use a `int256` and set it negative. Deposit amounts are limited to `uint128 - 1` . Cast to a `uint` when processing actual transfers.

To prevent double tap in the YieldManager, the off-chain caller to deploy is responsible for providing the integrations and pools to be updated:

```
deploy([{integration: address, tokens: address[], pool: uint256}]);
```

This allows the optimization of the deployment to be done cheaply off chain. It should order the updates such that all withdrawals occur first, followed by top ups. This would allow the deployer to save gas by skipping pools that don't need processing. 

## Assumptions:

- Strategies and Yield Management must deal with two types of integrations:
    - Single asset deposit types, such as Aave, and Yearn. These only require a single token type when entering a position. The current strategy implementation works with these by tracking expected balances and adjusting accordingly.
    - Dual asset deposit types, such as Uniswap V2, Uniswap V3, and SushiSwap. The current implementation doesn't properly handle these, and has no concept of an underlying pool, or the requirement to provide two tokens to enter the pool.
        - The current implementation ends up depositing user tokens across all accessible pools, according to weights in the integration that were set by an admin

### AMM Integrations

AMM Integrations must handle uneven deposit amounts, ie depositing 50 of token A, and 0 of token B. An AMM integration will take 2 tokens, no more no less.

- Handle uneven deposits by:
    - Deploying as much as possible to the AMM according to the constant product ratio at run time
    - Hold the excess in the integration for the next deploy, and to cover withdrawals from the integration
- AMM Integrations must inherit the `IAMMIntegration` interface
