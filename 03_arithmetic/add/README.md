## add1.asm

120 + 10 gave 130 (0x82). GDB displayed -126 because it
interpreted AL as a signed 8-bit value.

- CF = 0: 130 fits in the unsigned range of 0–255.
- PF = 1: 10000010 has two 1s, so parity is even.
- AF = 1: 8 + 10 in the lower four bits causes a carry into bit 4.
- ZF = 0: The result is not zero.
- SF = 1: The highest bit is 1.
- OF = 1: 130 exceeds the signed 8-bit maximum of 127.

## add2.asm

32000 + 500 gave 32500 (0x7EF4), stored in AX.
GDB showed EFLAGS = 0x202 [ IF ], so all six arithmetic
flags were cleared.

- CF = 0: 32500 fits in the unsigned 16-bit range of 0–65535.
- PF = 0: The lowest byte is F4 (11110100), which has five 1s, so parity is odd.
- AF = 0: The lowest four bits add as 0 + 4, with no carry into bit 4.
- ZF = 0: The result is not zero.
- SF = 0: The highest bit of the 16-bit result is 0.
- OF = 0: 32500 fits in the signed 16-bit range of -32768 to 32767.

IF was set in both programs, but ADD does not change it.