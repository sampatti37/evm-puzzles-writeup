# Level 1

To start, we run ```npx hardhat play``` as described in the original github README. When we run this we see the following puzzle

``` Assembly
############
# Puzzle 1 #
############

00      34      CALLVALUE
01      56      JUMP
02      FD      REVERT
03      FD      REVERT
04      FD      REVERT
05      FD      REVERT
06      FD      REVERT
07      FD      REVERT
08      5B      JUMPDEST
09      00      STOP

? Enter the value to send: (0)
```
All these puzzles will be written in evm byte code which is a compiled solidity smart contract. The goal of these puzzles it to enter the required value so the smart contract will run without hitting a ```REVERT``` code. 

In this example, we are asked to enter a value for the ```CALLVALUE``` byte code. This byte code represents the value of a given transaction. 
