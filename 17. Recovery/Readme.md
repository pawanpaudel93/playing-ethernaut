The easiest way to solve this level is to search the contract address on etherscan website and under the Internal Txns search for the latest contract created and here we find the lost contract address. And we can copy the SimpleToken contract to create a SimpleToken.sol and under the deploy and run transactions, interact with the lost contract by filling up the contract address at At Address. Then simply click the destroy button with your wallet address.

Another way to destroy contract to return all the ether is using web3.

```
functionJSONInterface={
    name: 'destroy',
    type: 'function',
    inputs: [{
        type: 'address',
        name: '_to'
    }]
}

params = [player]
lostTokenAddress=<Address Here>
data = web3.eth.abi.encodeFunctionCall(jsonInterface, params)
await web3.eth.sendTransaction({from: player, to: lostTokenAddress, data: data})
```

Contract addresses are deterministic and are calculated by keccack256(address, nonce) where the address is the address of the contract (or ethereum address that created the transaction) and nonce is the number of contracts the spawning contract has created (or the transaction nonce, for regular transactions).

address = sha3(rlp_encode(creator_account, creator_account_nonce))[12:]

This is how the contract address of the upcoming or already created contract can be determined.