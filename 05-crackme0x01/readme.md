In crackme0x01, the main function reads a 32-bit integer from standard input via scanf into the stack variable [rbp-0x4] and moves it into the EAX register (mov eax, DWORD PTR [rbp-0x4]). It then compares EAX against the hardcoded hexadecimal value 0x149a (cmp eax, 0x149a), jumping to the failure message if they do not match (jne 0x4011cf). Converting 0x149a to decimal via p/d 0x149a in pwndbg yields 5274, which satisfies the check and prints Password OK :).

<img width="1120" height="506" alt="Screenshot From 2026-11-22 19-39-35" src="https://github.com/user-attachments/assets/913e51a2-7f28-4f41-9409-442c891dded4" />
<img width="1119" height="192" alt="Screenshot From 2026-11-22 19-40-30" src="https://github.com/user-attachments/assets/e0ad3e69-d90b-4a99-a2b8-af4c35593fcb" />
