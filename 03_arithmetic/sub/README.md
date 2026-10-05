# Subtraction

I ran sub1.asm and sub2.asm in GDB and checked the flags
immediately after SUB, before the exit instructions.

## sub1.asm

50 - 80 gave -30, stored in AL as 0xE2 (11100010).
GDB showed EFLAGS = 0x287 [ CF PF SF IF ].

- CF = 1: 50 is less than 80, so unsigned subtraction needs a borrow.
- PF = 1: 11100010 has four 1s, so parity is even.
- AF = 0: The lowest four bits subtract as 2 - 0, so no borrow is needed between bits 3 and 4.
- ZF = 0: The result is not zero.
- SF = 1: The highest bit of the 8-bit result is 1.
- OF = 0: -30 fits in the signed 8-bit range of -128 to 127.

## sub2.asm

1000 - 2000 gave -1000, stored in AX as 0xFC18.
GDB showed EFLAGS = 0x287 [ CF PF SF IF ].

- CF = 1: 1000 is less than 2000, so unsigned subtraction needs a borrow.
- PF = 1: The lowest byte is 18 in hexadecimal (00011000), which has two 1s, so parity is even.
- AF = 0: The operands are 0x03E8 and 0x07D0. Their lowest four bits subtract as 8 - 0, so no borrow is needed between bits 3 and 4.
- ZF = 0: The result is not zero.
- SF = 1: The highest bit of the 16-bit result is 1.
- OF = 0: -1000 fits in the signed 16-bit range of -32768 to 32767.

Both results are negative, but neither causes signed overflow.
CF indicates an unsigned borrow, while OF indicates signed overflow.

IF was set in both runs, but SUB does not change it.