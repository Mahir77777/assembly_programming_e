# 03.asm — Saving a Return Address

The program starts at `_start` and puts 10 in RAX. When it runs `call calculate`, the CPU saves the address of the next instruction, labelled `after_call`, on the stack. The return address takes 8 bytes, so RSP moves down by 8 bytes before execution jumps to `calculate`.

Inside `calculate`, `add rax, 5` changes RAX from 10 to 15. The addition leaves RSP where it is, so the return address stays at the top of the stack. Then `ret` takes that address off the stack, moves RSP up by 8 bytes, and jumps to `after_call`.

Back at `after_call`, `nop` leaves the registers and stack alone. The program then puts 60 in RAX, clears RDI, and uses `syscall` to exit with status 0.

## Stack changes

| After this instruction | RSP relative to the start | What happened |
| --- | --- | --- |
| `mov rax, 10` | 0 bytes | RAX holds 10. |
| `call calculate` | −8 bytes | The return address is at the top of the stack. |
| `add rax, 5` | −8 bytes | RAX holds 15, and the return address is still on top. |
| `ret` | 0 bytes | The CPU takes the return address off the stack and jumps to `after_call`. |
| `nop` and exit setup | 0 bytes | RSP stays at its starting position. |

## Test observations

During the instruction trace, the top of the stack held `0x40100a` inside `calculate`. After `ret`, execution resumed at that same address, RAX held 15, and RSP was back at its starting position. The program printed nothing and exited with status 0.