# Division

I ran div1.asm and div2.asm in GDB and checked the registers
and flags immediately after DIV.

## div1.asm

100 divided by 7 gave:
- AL = 14 (quotient)
- AH = 2 (remainder)

This is correct because 14 × 7 + 2 = 100.

GDB showed EFLAGS = 0x212 [ AF IF ].

## div2.asm

The dividend was DX:AX, with DX = 0 and AX = 50000.
Dividing by 300 gave:
- AX = 166 (quotient)
- DX = 200 (remainder)

This is correct because 166 × 300 + 200 = 50000.

GDB showed EFLAGS = 0x212 [ AF IF ].

## Flags in both programs

The observed values were:

- CF = 0
- PF = 0
- AF = 1
- ZF = 0
- SF = 0
- OF = 0

However, CF, PF, AF, ZF, SF and OF are all undefined after DIV.
The instruction does not guarantee their values, so the observed
set or cleared states cannot be explained from the quotient or
remainder. For example, AF = 1 does not indicate a carry or borrow
caused by this division.

The results must be checked using the quotient and remainder
registers instead.

IF was also set, but DIV does not change it.