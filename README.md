# Securem-101-audit-findings

## 1. Unhandled return values of transfer and transferFrom: 
  ERC20 implementations are not always consistent. Some implementations of transfer and transferFrom could return ‘false’ on failure instead of reverting. 
  It is safer to wrap such calls into require() statements to these failures.
  Recommendation: Check the return value and revert on 0/false or use OpenZeppelin’s SafeERC20 wrapper functions.
  
  
## 2. Reentracy attack
  
  
## 3. Tokens with more than 18 decimal points will cause issues: It is assumed that the maximum number of decimals for each token is 18. 
  However uncommon, it is possible to have tokens with more than 18 decimals, as an example YAMv2 has 24 decimals. 
  This can result in broken code flow and unpredictable outcomes

  Recommendation: Make sure the code won’t fail in case the token’s decimals is more than 18

  Major severity finding from Consensys Diligence Audit of Defi Saver
  
  
## 4. Error codes of Compound’s Comptroller.enterMarket, Comptroller.exitMarket are not checked: 
  Compound’s enterMarket/exitMarket functions return an error code instead of reverting in case of failure. 
  DeFi Saver smart contracts never check for the error    codes returned from Compound smart contracts.

  Recommendation: Caller contract should revert in case the error code is not 0
  
## 5. Reversed order of parameters in allowance function call: 
  the parameters that are used for the allowance function call are not in the same order that is used later in the call to safeTransferFrom.

  Recommendation: Reverse the order of parameters in allowance function call to fit the order that is in the safeTransferFrom function call.
  
## 6. Token approvals can be stolen in DAOfiV1Router01.addLiquidity(): 
  DAOfiV1Router01.addLiquidity() creates the desired pair contract if it does not already exist, then transfers tokens into the pair and calls
  DAOfiV1Pair.deposit(). There is no validation of the address to transfer tokens from, so an attacker could pass in any address with nonzero token approvals
  to DAOfiV1Router. This could be used to add liquidity to a pair contract for which the attacker is the pairOwner, allowing the stolen funds to be retrieved
  using DAOfiV1Pair.withdraw().

  Recommendation: Transfer tokens from msg.sender instead of lp.sender
  
  
  ## 7. swapExactTokensForETH checks the wrong return value: 
  Instead of checking that the amount of tokens received from a swap is greater than the minimum amount expected from this swap, it calculates the difference
  between the initial receiver’s balance and the balance of the router

  Recommendation: Check the intended values
  
