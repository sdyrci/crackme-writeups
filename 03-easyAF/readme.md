During my static and dynamic analysis of this C++ binary, I first ran `info functions` in Pwndbg to enumerate the symbols, identifying the `main` function at `0x11f9` alongside standard `std::basic_string` operations, `std::operator>>` for input, and `std::operator==` for comparison.
When I disassembled `main` using `disas main`, I observed that after setting up the stack canary via `fs:0x28`, the program initializes a `std::string` at `[rbp-0x60]` and loads a hardcoded string pointer from `[rip+0xddb]` (`0x2005`) into `$rsi` right before calling `std::basic_string::operator=(char const*)@plt` at `main+52` (`0x122d`), followed by reading my input into `[rbp-0x40]` via `std::cin`.
I then set a breakpoint at `b *main+52` (`0x122d`) and inspected the source register with `x/s $rsi`, which directly revealed the hardcoded password `"pass"` at `0x555555556005`.
Finally, I continued execution with `c` and entered `"pass"` at the prompt, receiving the `"Welldone!"` message and confirming my solution.

<img width="1390" height="708" alt="Screenshot From 2026-11-20 16-09-53" src="https://github.com/user-attachments/assets/24f9c178-7d06-4153-a055-2824c4a8fa44" />
<img width="1414" height="588" alt="Screenshot From 2026-11-20 16-10-23" src="https://github.com/user-attachments/assets/8363019a-e04b-42b6-88b5-c52e00aab6b2" />
<img width="1059" height="51" alt="Screenshot From 2026-11-20 16-13-32" src="https://github.com/user-attachments/assets/9d990e58-8c34-4b84-9fba-db91bbf54d15" />
<img width="1582" height="126" alt="Screenshot From 2026-11-20 16-13-12" src="https://github.com/user-attachments/assets/a0113452-c995-487e-bf65-162d0cef2d15" />

