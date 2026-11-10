# 05.asm — Pushing and Restoring 64-Bit Values

The program starts by loading `0x1122334455667788` into RAX and `0x1234` into RBX. These two instructions leave RSP in the same place.

`push rax` moves RSP down by 8 bytes and stores the RAX value at the top of the stack. `push rbx` moves RSP down another 8 bytes and puts the RBX value on top. Even though `0x1234` is a small number, pushing RBX takes a full 8-byte slot. The `nop` leaves both values where they are.

`pop rbx` takes `0x1234` off the top of the stack and moves RSP up by 8 bytes. Then `pop rax` takes `0x1122334455667788` off the stack and moves RSP up another 8 bytes. The values come off in reverse order, so each one goes back into its original register and RSP returns to its starting position.

The program then puts 60 in RAX, clears RDI, and uses `syscall` to exit with status 0.

## Stack changes

| After this instruction | RSP relative to the start | What happened |
| --- | --- | --- |
| Both initial `mov` instructions | 0 bytes | The two values are in RAX and RBX. |
| `push rax` | −8 bytes | `0x1122334455667788` is on top. |
| `push rbx` and `nop` | −16 bytes | `0x0000000000001234` is on top, with the RAX value underneath. |
| `pop rbx` | −8 bytes | RBX gets `0x1234`. The RAX value is back on top. |
| `pop rax` | 0 bytes | RAX gets its original value, and RSP is back where it started. |

## Test observations

The instruction trace showed two 8-byte slots after the pushes. The slot at RSP held `0x0000000000001234`, and the next slot held `0x1122334455667788`. After the two pops, both registers held their original values and RSP was back at its starting position. The program printed nothing and exited with status 0.