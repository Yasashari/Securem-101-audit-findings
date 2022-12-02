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
  
  
