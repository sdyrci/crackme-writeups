Upon analyzing the disassembly, main reads the input and passes it with envp to check, which sums the digit values of the characters until reaching 16 (0x10) before invoking parell. The parell function parses the full input as an integer, verifies via dummy that an environment variable starts with "LOL" (0x402004), and checks if the number is even (num & 1 == 0) to print "Password OK!" (0x40200c) at 0x4012ad. While setting LOLO=1 and entering 88 satisfies the logic, all checks were bypassed in pwndbg by breaking right before the password prompt at 0x40139d and redirecting $rip directly to 0x4012ad (set $rip = 0x4012ad).

<img width="890" height="534" alt="Screenshot From 2026-11-23 15-51-10" src="https://github.com/user-attachments/assets/2a4bd9d4-ab18-41d0-9990-89cfbe0a83a0" />
<img width="758" height="879" alt="Screenshot From 2026-11-23 15-51-23" src="https://github.com/user-attachments/assets/27c7b947-f4d5-4893-8fb0-38c5327fb184" />
<img width="686" height="763" alt="Screenshot From 2026-11-23 15-57-59" src="https://github.com/user-attachments/assets/84360e7b-afda-468f-a8ca-27273fd3f655" />
<img width="785" height="563" alt="Screenshot From 2026-11-23 15-51-36" src="https://github.com/user-attachments/assets/517cb8d9-b1d1-4d2c-bfbf-b51f72c7e967" />
<img width="367" height="93" alt="Screenshot From 2026-11-23 15-55-37" src="https://github.com/user-attachments/assets/bbf086e5-5ba8-49c1-8c2c-27d20311acc3" />
<img width="785" height="110" alt="Screenshot From 2026-11-23 15-55-15" src="https://github.com/user-attachments/assets/ef8719a2-3e0f-469a-81d9-f4a7a4d508d8" />
