Issues:
Everything needs to pass through the buffers, nothing can go from the control unit directly. Only one case of this left, the selector after the alu. 


TASKS

| Task              | Status |
|-------------------|--------|
| IR                | DONE   |
| FETCH             | DONE   |
| DECIDE ON OPCODES | 100%   |
| CU                | 50%    |
| ALU               | DONE   |
| STORE             | DONE   |
| MULT              | DONE   |
| SUB               | 100%   |
| COMP              | 100%   |
| ADD               | 100%   |
| STOP              | 100%   |
| HALT/ON-OFF       | 0%     |
| BRANCH            |


## Stage 1: Retrieve one instruction from cache ##

    1. PUSH PC -> FETCH stage
    2. SEND (PC) -> CACHE
    3. (CACHE[PC]) -> IR
## Stage 2: Get instruction's arguments (from Data Cache, GP Register, or Keypad data register) ##

    1. Decode IR
        - IR[0-2] -> CU
            - LOAD
                - IR[3] -> FETCH
                - IR[4-7] -> FETCH
            - R-TYPE
                - IR[3] -> FETCH
                - IR[4] -> FETCH
            - JUMP / MOVE
                - IR[4-7] -> IR
    2. Load values
        - R-Type
            - FETCH[0] -> ALU-L
            - FETCH[1] -> ALU-R

## Stage 3: Execute the instruction (eg ALU operation, JUMP or MOVE) ##
    1. R-Type
        CU-> ALU-OP
    2. JUMP / MOVE
         IR -> PC

## Stage 4: Save the result (if any, into Data Cahce, GP Registers or display buffer register) ## ##
    1. Use OUTPUT[CU]
    2. ALU -> OUTPUT

INSTRUCTION FORMATS:

| OPCODE | FORMAT           | Bit |            |
| :----- | :--------------- | --- | ---------- |
| STOP   | [0-2][3][4]      | 011 | NEEDS HALT |
| STORE  | [0-2][3][4-7]    | 001 | DONE       |
| LOAD   | [0-2][3][4-7]    | 010 | DONE       |
| PRINT  | [0-2][3]         | 100 | DONE       |
| READ   | [0-2][3]         | 011 | DONE       |
| NOP    | [0-2]            | 000 | DONE       |
| BRANCH | [0-2][3][4-7]    | 101 |            |
| ALU    | [0-2][3][4][5-7] | 111 | DONE       |


| FUNCTIONAL FIELD |     |
| ADD              | 001 |
| SUB              | 010 |
| COMP             | 011 |
| NOP              | 000 |

| OPTIONAL INSTRUCTIONS   ||
|:---------|:--------------|
| MUL      |               |
| COMP     |               |
| SUB      |               |






##COMPONENTS

| Quantity |            Type           | Input Bits | Output Bits |
|----------|---------------------------|------------|-------------|
|        2 | GP Registers              |          8 |           8 |
|        1 | IR                        |          8 |           8 |
|        1 | 16 Byte instruction cache |          0 |           8 |
|        1 | 16 Byte Data Cache        |          8 |           8 |
|        1 | CU                        |          3 |           3 |
|        1 | PC                        |          4 |           4 |
|        1 | Display Register          |          8 |           8 |
|        1 | Keypad + Register         |          0 |           0 |

###Control Unit
Inputs: 3 bit opcode
Outputs: ALU code, register?, ramwrite?,branch?

### Multiplier ###
implemented in multiplication.circ
Booth 2 Encoded Wallace Tree

Parts: 
Booth Encoder: Done.
Partial Product: Done.
Carry Save Adder: Used Library

The multiplier works for all inputs except -128 since there is no way to represent 128 in a byte in 2's complement form. This should be solvable through a sign extend from 8-9 bits and then doing 2's complement. Whether this is worth it is up for debate. Another option is simply refusing to multiply by -128 limiting the range to [-127,127]. If the user attempts to do it we can raise an error or something along those lines. 



