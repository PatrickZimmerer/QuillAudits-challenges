# MetaToken

## Description

There is a new metatoken in the market. Alice and Bob have already bought it. Don't miss out on it

## Objective of CTF

Drain as much eth+weth you can from every contract and meet the assertions

### Solution

- Since the balances of from & to are cached in the first lines of the transfer function we can double our balance by sending our complete balance to ourself this will result in frombalance & tobalance being 100, at the end we subtract the amount by the chached value in this line `_balances[_from] = frombalance - _amount;` after that we add the amount to our cached `tobalance` which will be 100 + 100 => 200 in this piece of code `_balances[_to] = tobalance + _amount;`

### Steps

#### lpToken / CurveToken

- Owner == deployer
- minter == pool => pool can mint, mint_relative & burnFrom

#### swapPoolEthWeth / CurvePool

- Owner == deployer
- curvetoken == lptoken
- initial_A == future_A // weird => value: 500
- coins are wETH & eth (burn address)
- fee == 4000000
- admin_fee = 5000000000

#### metaToken / MetaPoolToken

- mint & burn rely on get_virtual_price(), when burning / minting you receive / send lp token to the MetaPoolToken

#### starting conditions

- Alice get's 20 ether, trades 10 eth for 10 wrapped eth, approves the pool for wETH uint256.max and adds liquidity (10eth & wETH)
- Alice approves the metaToken contract for lpToken uint256.max and mints metatoken passing in her balance of lpToken

- Bob get's 40 ether, trades 20 eth for 20 wrapped eth, approves the pool for wETH uint256.max and adds liquidity (20eth & wETH)
- Bob approves the metaToken contract for lpToken uint256.max and mints metatoken passing in his balance of lpToken

- lending pool get supplied with a LOT of liquidity => you can flashLoan a TON of wETH => maybe we can perform a inflation attack?

- We start with 10 eth

- We can't steal from the wethLendingPool
- We can't coompletely drain the lpToken which are stored in the metaToken contract
- We shouldn't obtain ~ 200x as much ETH as the final weth

### Attacker Contract

- Not needed

### Test

```solidity
 function testFactory() public {
        vm.prank(user1);
        _factory.transfer(user1, user1, 100);

        uint256 newbalance = _factory.checkbalance(user1);
        assertEq(newbalance, 200);
    }
```
