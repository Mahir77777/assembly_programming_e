# 01.asm — Push and Pop

The program starts by putting 100 in RAX and 200 in RBX. It then uses the stack to move those values into two other registers.

First, `push rax` moves RSP down by 8 bytes and puts 100 at the top of the stack. Then `push rbx` moves RSP down another 8 bytes and puts 200 on top of 100. The `nop` instruction leaves the stack as it is.

Next, `pop rcx` takes 200 from the top of the stack and moves RSP up by 8 bytes. Then `pop rdx` takes 100 and moves RSP up by another 8 bytes. RSP is now back at its starting address. This shows how the stack works in last-in, first-out order. The value pushed last is the first one popped.

The program then uses the Linux exit syscall to finish with status 0.

## What I observed

| Point in the program | RSP | What I saw |
| --- | --- | --- |
| At the start | `0x7fffffffdd20` | Starting stack address |
| After `push rax` | `0x7fffffffdd18` | 100 was on top |
| After `push rbx` | `0x7fffffffdd10` | 200 was on top, with 100 underneath |
| After `pop rcx` | `0x7fffffffdd18` | RCX held 200, and 100 was back on top |
| After `pop rdx` | `0x7fffffffdd20` | RDX held 100, and RSP was back where it started |

When I ran the program in the terminal, it printed nothing. I used `echo $?` to check the exit status and it showed 0.
