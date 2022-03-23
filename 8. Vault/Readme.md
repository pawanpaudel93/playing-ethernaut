Slot 0 occupies bool locked and as password is bytes32 it cannot be stored at slot 0 so it is stored at slot 1 and to get the password at slot 1 and unlock the contract we can run this command in the console. Everything on blockchain can be read even if its private.

```
await contract.unlock(await web3.eth.getStorageAt(contract.address, 1));
```