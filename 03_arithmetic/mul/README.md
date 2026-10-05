# Multiplication

I ran mul1.asm and mul2.asm in GDB and checked the registers
and flags immediately after MUL.

## mul1.asm

25 × 10 gave 250 (0x00FA), stored in AX.
AH was 0 and AL was 0xFA.

GDB showed EFLAGS = 0x202 [ IF ].

- CF = 0: AH is zero, so the product fits in the lower 8 bits.
- OF = 0: MUL clears OF when the upper half of the result is zero.
- PF, AF, ZF and SF: Undefined after MUL. GDB showed them as 0,
  but these values cannot be relied on or explained from the product.

GDB displayed AL as -6 because it interpreted that byte as signed.
MUL is unsigned, so 0xFA represents 250 in this operation.

## mul2.asm

3000 × 200 gave 600000 (0x000927C0).
The result was split between DX and AX:

- DX = 0x0009 = 9
- AX = 0x27C0 = 10176

The full result is (9 × 65536) + 10176 = 600000.

GDB showed EFLAGS = 0xa03 [ CF IF OF ].

- CF = 1: DX is not zero, so the product does not fit in the lower 16 bits.
- OF = 1: MUL sets OF when the upper half of the result is not zero.
- PF, AF, ZF and SF: Undefined after MUL. They appeared cleared
  in this run, but MUL does not guarantee their values.

For MUL, CF and OF indicate whether the upper half of the product
is nonzero. The full product is still stored correctly.

IF was set in both runs, but MUL does not change it.