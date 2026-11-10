# 04.asm — A 32-Bit Stack Frame

The program starts at `_start` and calls `work`. Because this is a 32-bit program, `call work` puts a 4-byte return address on the stack and moves ESP down by 4 bytes.

Inside `work`, `push ebp` moves ESP down another 4 bytes and saves the old EBP. Then `mov ebp, esp` makes EBP point to that saved value. EBP gives the function a fixed place to measure from while `sub esp, 8` makes room for two 4-byte local values.

The program uses `mov` to store 100 at `[ebp - 4]` and 200 at `[ebp - 8]`. It then reads those values into EAX and EBX. The stores, loads, and `nop` all leave ESP in the same place.

To clean up, `mov esp, ebp` brings ESP back to the saved EBP and releases the space for the local values. `pop ebp` restores the old EBP and moves ESP up by 4 bytes. Then `ret` takes the return address off the stack, moves ESP up another 4 bytes, and jumps back to the instruction after `call work`. The program sets EAX to 1 and EBX to 0, then uses `int 0x80` to exit with status 0.

## Stack changes

| After this instruction | ESP relative to the start | What happened |
| --- | --- | --- |
| `call work` | −4 bytes | The return address is on top of the stack. |
| `push ebp` | −8 bytes | The old EBP is on top, with the return address underneath. |
| `mov ebp, esp` | −8 bytes | EBP points to the saved EBP. The return address is at `[ebp + 4]`. |
| `sub esp, 8` | −16 bytes | Two 4-byte slots are reserved for local values. |
| Stores, loads, and `nop` | −16 bytes | `[ebp - 4]` holds 100 and `[ebp - 8]` holds 200. EAX and EBX receive those values. |
| `mov esp, ebp` | −8 bytes | ESP points back to the saved EBP. |
| `pop ebp` | −4 bytes | The old EBP is restored, and the return address is back on top. |
| `ret` | 0 bytes | Execution resumes at the instruction after `call work`. |

## Test observations

In the 32-bit emulator, ESP reached 16 bytes below its starting position. The two local slots held 100 and 200, and the loads copied them into EAX and EBX. The saved return address was `0x08049005`. After `ret`, execution resumed there and ESP was back at its starting position. The program printed nothing and exited with status 0.