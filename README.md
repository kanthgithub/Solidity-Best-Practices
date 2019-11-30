# Solidity-Best-Practices
Solidity-Best-Practices Collection

## Design Patterns:

## State Machine:

- https://blog.tokenfoundry.com/a-solidity-implementation-of-the-state-machine-design-pattern/

## CRUD:

- https://medium.com/@robhitchens/solidity-crud-part-1-824ffa69509a

- https://medium.com/@robhitchens/solidity-crud-part-2-ed8d8b4f74ec


## Storage:

- Referential Integrity:

   - One-Many Joins

      - https://medium.com/@robhitchens/enforcing-referential-integrity-in-ethereum-smart-contracts-a9ab1427ff42

      - https://bitbucket.org/rhitchens2/soliditystoragepatterns
      
      - https://blog.b9lab.com/storage-pointers-in-solidity-7dcfaa536089
      
      - https://ethereum.stackexchange.com/questions/13167/are-there-well-solved-and-simple-storage-patterns-for-solidity
      
      - https://ethereum.stackexchange.com/questions/13167/are-there-well-solved-and-simple-storage-patterns-for-solidity

## Strings:

   https://blog.aventus.io/working-with-strings-in-solidity-473bcc59dc04
   
## Loops:

   https://blog.b9lab.com/getting-loopy-with-solidity-1d51794622ad
   
## Vulnerabilities and their detection

   https://hackernoon.com/ethernaut-lvl-0-walkthrough-abis-web3-and-how-to-abuse-them-d92a8842d71b
   
   https://medium.com/nomic-labs-blog/malicious-backdoors-in-ethereum-proxies-62629adf3357
   
## Transactions

   https://ethereum.stackexchange.com/questions/39360/how-to-call-my-contracts-function-using-sendrawtransaction

## Basics:

- Calls vs Transactions

  https://blog.b9lab.com/calls-vs-transactions-in-ethereum-smart-contracts-62d6b17d0bc2

- Correct Way to Send Ether

  https://medium.com/@kidinamoto/how-to-send-ether-correctly-a60208ad76d9

- Fanout smart contract payments to save on gas costs

  https://github.com/Arachnid/PayToMany
  
- Eth OPCodes Summary (Useful for getting sense of code to instruction mapping)

  https://ethereum.stackexchange.com/questions/119/what-opcodes-are-available-for-the-ethereum-evm
  
  
 ## Consensys References:
 
 - https://www.consensys.university/resources/
 
 ## Create Contract in a Contract:
 
 - https://ethereum.stackexchange.com/questions/5676/create-new-contract-via-call?rq=1
 
 ```
 Here's an example of how to use the create opcode:
```

``js
contract Factory {
    function create(bytes code) returns (address addr){
        assembly {
            addr := create(0,add(code,0x20), mload(code))
            jumpi(invalidJumpLabel,iszero(extcodesize(addr)))
        }
    }
}

contract Adder {
    function add(uint a, uint b) returns (uint){
        return a+b;
    }
}

contract Tester {
    Adder a;

    function Tester(address factory){
        a = Adder(Factory(factory).create(
        hex"606060405234610000575b60ad806100186000396000f30060606040526000357c0100000000000000000000000000000000000000000000000000000000900463ffffffff168063771602f714603c575b6000565b34600057605d60048080359060200190919080359060200190919050506073565b6040518082815260200191505060405180910390f35b600081830190505b929150505600a165627a7a723058205d7bec00c6d410f7ea2a3b03112b597bb3ef544439889ecc1294a77b85eab15e0029"
            ));
        if(address(a) == 0) throw;
    }

    function test(uint x, uint y) constant returns (uint){
        return a.add(x,y);
    }
}
``

```
Just deploy the Factory, pass its address into the Tester, and the Tester will create a new Adder, which it will use to add together integers passed to the test function.

The factory will throw if the create failed.
 ```
