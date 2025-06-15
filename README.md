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
<index> <operand>      <info>             <operands>

 00000    NOOP     No operation                0
 00001    LDI      Load immediate              1
 00010    RST      Update register             1
 00011    RLD      Read register               1
 00100    PST      Update port                 1
 00101    PLD      Read port                   1
 00110    LOD      Update ram                  1
 00111    STR      Read ram                    1
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

# Programming

.