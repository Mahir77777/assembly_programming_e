# 02.asm — Using the Stack for Local Values

The program starts at `_start` and runs `call work`. The call puts the return address on the stack and lowers RSP by 8 bytes. That address tells the program where to continue after `work` finishes.

Inside `work`, `push rbp` saves the old RBP and lowers RSP by another 8 bytes. Then `mov rbp, rsp` gives the function a fixed point to measure from. `sub rsp, 16` makes room for two 8-byte values on the stack.

The function uses `mov` to store 100 at `[rbp - 8]` and 200 at `[rbp - 16]`. It then reads them back into RAX and RBX. Reading and writing those slots leaves RSP at the same address.

To clean up, `mov rsp, rbp` releases the 16 bytes reserved for the local values. Then `pop rbp` restores the old RBP and raises RSP by 8 bytes. Finally, `ret` takes the return address off the stack, raises RSP by another 8 bytes, and jumps back to `_start`. The program then uses the Linux exit syscall with status 0.

## What I observed in GDB

| Point in the program | What I saw |
| --- | --- |
| At the start | RSP was `0x7fffffffdd20`. |
| After `call work` | RSP was `0x7fffffffdd18`, and the top of the stack held `0x401005`. |
| After making room for the local values | RBP was `0x7fffffffdd10` and RSP was `0x7fffffffdd00`. |
| After storing and reading the values | The stack slots held 100 and 200. RAX received 100 and RBX received 200. |
| After `mov rsp, rbp` | RSP was back at `0x7fffffffdd10`. |
| After `ret` | Execution resumed at `0x401005`, and RSP returned to `0x7fffffffdd20`. |
