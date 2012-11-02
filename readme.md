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

|  OPCODE  |     FORMAT    |
|:---------|:--------------|
| ADD      |  [0-2][3][4]  |
| STORE    | [0-2][3][4-7] |
| LOAD     | [0-2][3][4-7] |
| PRINT    | [0-2][3]      |
| READ     | [0-2][3]      |
| STOP     | [0-2]         |
| BRANCH   | [0-2][3][4-7] |
| JUMP     | [0-2][3-6]    |


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
Outputs: ALU code, ALU?, Store?

### Multiplier ###
Booth Encoded Wallace Tree

Parts: 
Booth Encoder
Partial Product
Carry Save Adder



