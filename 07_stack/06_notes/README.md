# 06.asm — Pushing and Restoring 32-Bit Values

The program starts by loading `0x11223344` into EAX and `0x1234` into EBX. These two instructions leave ESP in the same place.

`push eax` moves ESP down by 4 bytes and puts the EAX value on top of the stack. Then `push ebx` moves ESP down another 4 bytes and puts the EBX value on top of it. Each push uses 4 bytes because the program is pushing 32-bit registers. The `nop` leaves the stack alone.

`pop ebx` takes `0x1234` off the top and moves ESP up by 4 bytes. Then `pop eax` takes `0x11223344` off the stack and moves ESP up another 4 bytes. Both registers have their original values again, and ESP is back at its starting position.

The program sets EAX to 1 and clears EBX to 0. It then uses `int 0x80` to exit with status 0.

## Stack changes

| After this instruction | ESP relative to the start | What happened |
| --- | --- | --- |
| Both initial `mov` instructions | 0 bytes | The two values are in EAX and EBX. |
| `push eax` | −4 bytes | `0x11223344` is on top. |
| `push ebx` and `nop` | −8 bytes | `0x00001234` is on top, with the EAX value underneath. |
| `pop ebx` | −4 bytes | EBX gets `0x1234`. The EAX value is back on top. |
| `pop eax` | 0 bytes | EAX gets its original value, and ESP is back where it started. |

## Test observations

The 32-bit emulator showed that the two pushes used 8 bytes in total. At the lowest point, the slot at ESP held `0x00001234`, and the next slot held `0x11223344`. The two pops restored both values and returned ESP to its starting position. The program printed nothing and exited with status 0.