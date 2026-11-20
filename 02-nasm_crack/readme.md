Before diving into the debugger, I started with basic static reconnaissance using `file` and `strings` to gather initial clues about the binary. Running `file` revealed that `nasm_crack` is a statically linked, non-stripped 64-bit ELF executable written in NASM assembly, meaning all symbol labels remain intact and Linux system calls are invoked directly without relying on external `libc` functions.

<img width="1111" height="53" alt="Screenshot From 2026-11-20 15-01-18" src="https://github.com/user-attachments/assets/ecb43812-3205-423f-82ac-8347b225be86" />

Next, inspecting the binary with `strings` exposed the `.data` section strings alongside the internal symbol names (`_start`, `correct_func`, `passwd`, `input`). Alongside the standard prompt and feedback strings (`"Enter your password:"`, `"Correct!"`, `"Wrong!"`), the plaintext string `"supersecret"` immediately stood out as the hardcoded password candidate.

<img width="1123" height="465" alt="Screenshot From 2026-11-20 15-01-37" src="https://github.com/user-attachments/assets/182882b0-bd05-4b5a-9560-ac007bf3a221" />

Since the binary was not stripped, I loaded it into `pwndbg` and disassembled `correct_func` (`0x401000`) to see what happens upon successful validation. The assembly dump confirmed that this routine simply invokes `sys_write` (`eax = 0x1`) to print `"Correct!"` from `0x402016` and terminates the process cleanly via `sys_exit` (`eax = 0x3c`).

<img width="1123" height="248" alt="Screenshot From 2026-11-20 15-13-44" src="https://github.com/user-attachments/assets/d1de0e1a-d848-46a8-9bd0-d7c4d342012a" />

Disassembling `_start` (`0x401028`) mapped out the core execution flow instruction by instruction. After printing the prompt via `sys_write` and reading up to 16 bytes of user input into `0x402031` (`input`) via `sys_read`, the program loads `0x402026` (`passwd`) into `RDI`, `0x402031` (`input`) into `RSI`, and `0xb` (11) into `ECX`. It then executes `repz cmps BYTE PTR [rsi], BYTE PTR [rdi]` at `0x401077` to compare the strings byte by byte, jumping to `correct_func` (`0x401000`) if all 11 bytes match.

<img width="1182" height="524" alt="Screenshot From 2026-11-20 15-13-57" src="https://github.com/user-attachments/assets/304bc209-204d-4234-b3db-bbf685ed98f5" />

To verify the comparison behavior dynamically, I placed a breakpoint at `0x401077` (`b *0x401077`) and ran the program with a test input (`attempt`). When execution paused at the breakpoint, the register view confirmed that `RDI` pointed directly to `"supersecret"` while `RSI` held my input (`"attempt\n"`). Inspecting `RCX` (`p $rcx`) verified the counter value of `0xb` (11), which matches the exact 11-character length of `"supersecret"` and neatly ignores the trailing newline (`\n`) captured by `sys_read`, confirming `"supersecret"` as the valid password.

<img width="1417" height="50" alt="Screenshot From 2026-11-20 15-14-10" src="https://github.com/user-attachments/assets/815efcbf-8503-4c15-ab0b-e6d675f72191" />
<img width="1390" height="976" alt="Screenshot From 2026-11-20 15-14-46" src="https://github.com/user-attachments/assets/bd8faa73-f9f0-4feb-90e1-c69476d70183" />
<img width="1390" height="48" alt="Screenshot From 2026-11-20 15-15-03" src="https://github.com/user-attachments/assets/effadff8-5202-4a5c-891d-9aac8c6e549e" />

