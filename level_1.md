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

In order to figure out what we need to enter, we need to know how ```CALLVALUE``` works. This reference (https://www.evm.codes/?ref=hackernoon.com&fork=merge) has all the evm byte code and explains what it does. 

Looking at the description for ```CALLVALUE``` we see that it takes a value and pushes it to the top of the stack. Lets say we have a new contract with a stack like this

``` Assembly
[ 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0]
```
and we enter a value of ```10``` to the ```CALLVALUE``` instruction. Our stack would then look like this

``` Assembly
[ 10 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0]
```
since that value will be pushed to the top of the stack. 

We see that the next instruction is ```JUMP```. Looking at the man page for the evm op codes, we see that ```JUMP``` takes the value on the top of the stack and jumps to that many bytes in the instruction code. For the evm, each op code is 1 byte. So in the example from before, if 10 was pushed onto the stack by ```CALLVALUE```, then ```JUMP``` would take that value and move the program counter to the op code at address 0xa.

The only rule with ```JUMP``` statements is that it must land on a ```JUMPDEST``` op code.

In puzzle 1, the ```JUMPDEST``` instruction is at address 0x8. This means that the first value on the stack must be 0x8. In order to put this value on the stack, we need to pass it to ```CALLVALUE```. This means that if we enter ```8``` as the answer, it should work.

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

? Enter the value to send: 8

Puzzle solved!
```
Nice Work!
