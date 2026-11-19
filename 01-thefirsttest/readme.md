
Before launching the debugger, I performed a quick static reconnaissance on the binary using `strings` combined with `grep -iE` to filter out noise and locate interesting keywords. This immediately revealed the input prompts (`"Please input the password:"`) along with the success and failure messages (`"Well done, you are a master hacker!"` and `"Wrong password, try agian!"`), confirming that the validation logic relies on a simple conditional check.

<img width="1912" height="109" alt="Screenshot From 2026-11-20 01-51-32" src="https://github.com/user-attachments/assets/b6c5e20b-4485-4c0a-a5ad-b22e43cdd9f9" />

Since the actual password was not hardcoded as readable text, I loaded the executable into `pwndbg` to examine its internal structure. Running `info functions` listed all non-debugging symbols in the binary and showed that the core program logic resides inside the `main` function starting at memory address `0x400476`.

<img width="1912" height="558" alt="Screenshot From 2026-11-20 01-12-06" src="https://github.com/user-attachments/assets/ccabc7ed-6ae6-42ec-9bab-b2a0b3bd9ea9" />

Next, I disassembled `main` using `disas main` to trace the execution flow instruction by instruction. The assembly code shows that the program calls `__isoc23_scanf@plt` (`0x4004ad`) to read an integer from the user and stores it on the stack at `[rbp-0x4]`. Immediately afterward, it loads this value into the `eax` register and executes `cmp eax, 0x72abb3c9` at `0x4004b5`, followed by a conditional jump (`jne 0x4004c8`) that redirects execution to the error message block if the input does not match the expected hexadecimal value (`1923855305` in decimal).

<img width="1918" height="308" alt="Screenshot From 2026-11-20 01-11-49" src="https://github.com/user-attachments/assets/0b81ad16-d498-4e5d-95e2-7d592da68b5a" />

To observe this comparison in action, I set a breakpoint at `0x4004b5` (`main+63`) and ran the program with an arbitrary test input. When execution paused at the breakpoint, the register view confirmed that `EAX` held `0x0`, causing the `cmp` instruction to leave the Zero Flag (`ZF`) unset and triggering the `jne` jump toward `"Wrong password, try agian!"`.

<img width="1909" height="998" alt="Screenshot From 2026-11-20 01-19-11" src="https://github.com/user-attachments/assets/14958f14-faab-4f53-b055-3883eab3c907" />

Instead of restarting the binary with the converted decimal password, I chose to bypass the check dynamically at runtime. By executing `set $eax=0x72abb3c9`, I overwrote the register with the exact value expected by the comparison, and after resuming execution with `c`, the program bypassed the jump and printed `"Well done, you are a master hacker!"`.

<img width="1912" height="108" alt="Screenshot From 2026-11-20 01-19-33" src="https://github.com/user-attachments/assets/45f75830-a070-4c5d-9bcf-a398e905e9e8" />

