# 54PU

54pu is a minecraft CPU

# Table of contents
- [54PU](#54pu)
- [Table of contents](#table-of-contents)
- [Specifications](#specifications)
    - [Speed](#speed)
    - [Memory](#memory)
- [ISA](#isa)
    - [Opcodes](#opcodes)
    - [Flags](#flags)
- [Programming](#programming)

# Specifications

## Speed

Speed in redstone ticks (1 = 2 game ticks)

I/O ports input and output is synced.

```yaml
Clock-speed: ?

Register-acc: ?
Register-file: 4
Register-file-input-wire: 4
Register-file-output-wire: 3

ALU: 5
ALU-with-OR: 7

IO-ports: 5
```

## Memory

Memory in bytes (1 = 8 bits)
Exception (*not really*): registers and I/O ports are not counted in bytes and rather amount.

```yaml
IO-ports: 5
Registers: 8
RAM: ?
```

# ISA

## Opcodes

```
<index> <opcode>      <info>             <operands>

 00000    NOOP     No operation                0
 00001    LDI      Load immediate              1
 00010    RST      Update register             1
 00011    RLD      Read register               1
 00100    PST      Update port                 1
 00101    PLD      Read port                   1
 00110    STR      Update ram                  1
 00111    LOD      Read ram                    1
 01000    INC      Increment acc               0
 01001    DEC      Decrement acc               0
 01010    ADD      Add acc with reg            1
 01011    SUB      Sub acc with reg            1
 01100    OR       OR acc with reg             1
 01101    NOR      NOR acc with reg            1
 01110    XOR      XOR acc with reg            1
 01111    XNOR     XNOR acc with reg           1
 10000    AND      AND acc with reg            1
 10001    NAND     NAND acc with reg           1
 10010    HLT      Halt cpu                    0
 10011    JMP      Jump to instruction         1
 10100    BRC      Conditionally jump          2
 10101    ---      -------------------         -
 10110    ---      -------------------         -
 10111    ---      -------------------         -
 11000    ---      -------------------         -
 11001    ---      -------------------         -
 11010    ---      -------------------         -
 11011    ---      -------------------         -
 11100    ---      -------------------         -
 11101    ---      -------------------         -
 11110    ---      -------------------         -
 11111    ---      -------------------         -

```

## Flags

```
<index> <flag>    <info>

  00     NOOP   No operation
  01     ZERO   If output zero
  10     COUT   If ALU cout
  11     ----   --------------

```

## Operands
Instructions are 8 bit, sometimes you need 2 instructions to finish a task for example, with LDI you will first input the opcode as the first instruction `00001` then you will input the 8 bit immediate value in the next instruction `110110`

The result of the load immediate instruction will be `00001000, 110110`

Instructions are normally made with the `opcode` being the first 5 bits and then the `operand` being the last 3 bits. `00010` (RST, see [Opcodes](#opcodes)) + `010` (2)

# Programming

## Assembling

Instructions are in hex (max 15) because of the icache having 4 different latch output possibilities meaning, you can compress 4 instructions in 1 barrel.

Example:

```
00001000 (Load immediate of 54)
00110110
00010000 (Store in register 0)
00011001 (Read register 1)
```

Now you read each instruction bit by bit vertically:

```
0 | 0 | 0 | 0 | 1 | 0 | 0 | 0 (Load immediate of 54)
0 | 0 | 1 | 1 | 0 | 1 | 1 | 0
0 | 0 | 0 | 1 | 0 | 0 | 0 | 0 (Store in register 0)
0 | 0 | 0 | 1 | 1 | 0 | 0 | 1 (Read register 1)

|   |   |   |   |   |   |   |
v   v   v   v   v   v   v   v

0   0   4   7   9   4   4   1
```

And your instruction in hex would be `00479441`