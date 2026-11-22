In this 64-bit x86-64 challenge, main reads user input via scanf("%s") into a 128-byte stack buffer ([rbp-0x80]) and passes it to check, which parses each character into an integer using sscanf("%d") and accumulates a running total in [rbp-0x4] until the sum reaches 0x10 (16), at which point it forwards the input to a second validation function named parell; parell then parses the input as a single integer and checks its least significant bit via and eax, 0x1 to verify that the number is even (test eax, eax resulting in 0), printing "Password OK!" and calling exit(0) upon success, meaning any even number whose digits sum to 16 (such as 88 or 970) solves the challenge.


<img width="1229" height="424" alt="Screenshot From 2026-11-23 01-25-33" src="https://github.com/user-attachments/assets/f464bc0f-a6e1-4841-8fcc-db94bf0f0971" />
<img width="1210" height="830" alt="Screenshot From 2026-11-23 01-25-46" src="https://github.com/user-attachments/assets/726c50a6-68dd-416a-b7f3-cdd7428f1094" />
<img width="1210" height="471" alt="Screenshot From 2026-11-23 01-26-03" src="https://github.com/user-attachments/assets/df284d3b-2e1e-4c73-b869-13386c0233c4" />
<img width="1088" height="106" alt="Screenshot From 2026-11-23 01-26-29" src="https://github.com/user-attachments/assets/8b9acc8b-9800-4dba-860c-7359a9bcca93" />
<img width="1088" height="144" alt="Screenshot From 2026-11-23 01-29-27" src="https://github.com/user-attachments/assets/5a0a5e45-3c5b-468e-b59b-e23f78292216" />
