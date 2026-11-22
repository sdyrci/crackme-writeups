The main function reads a 32-bit integer from standard input via scanf into [rbp-0x10] and computes the expected password at runtime by adding two hardcoded constants (0x5a + 0x1ec = 0x246) and squaring the result (imul eax, eax), storing the final value in EDX before loading the user input into EAX. It then compares both registers (cmp edx, eax) and jumps to the failure message if they do not match (jne 0x4011ed). Setting a breakpoint at *main+101 in pwndbg and inspecting p/d $edx reveals the calculated password 338724, which satisfies the check

<img width="1120" height="723" alt="Screenshot From 2026-11-22 19-47-12" src="https://github.com/user-attachments/assets/65039b31-4efc-456f-b1b3-704d8b65a626" />
<img width="771" height="49" alt="Screenshot From 2026-11-22 19-49-00" src="https://github.com/user-attachments/assets/c1a7458a-2779-42fb-899f-83a3addd7e01" />
<img width="771" height="49" alt="Screenshot From 2026-11-22 19-49-10" src="https://github.com/user-attachments/assets/43b9ce09-8aba-4665-9254-060fa8141010" />
<img width="830" height="145" alt="Screenshot From 2026-11-22 19-49-26" src="https://github.com/user-attachments/assets/beadc58d-7b2a-4caf-be89-ac2c8c046afd" />
