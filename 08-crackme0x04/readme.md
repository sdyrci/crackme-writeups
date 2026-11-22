In this 64-bit x86-64 challenge, `main` reads user input via `scanf("%s")` into a 128-byte stack buffer (`[rbp-0x80]`) and passes it to `check`, which loops through each character, parses it as an integer with `sscanf("%d")`, and adds it to a running sum (`[rbp-0x4]`) until it hits `0xf` (15), printing `"Password OK!"` and calling `exit(0)`; thus, any input whose digits sum to 15 (such as `81231`) solves the challenge.

<img width="1229" height="425" alt="Screenshot From 2026-11-23 01-00-14" src="https://github.com/user-attachments/assets/da39cee9-b40c-47ae-97ae-3f36a3555778" />
<img width="1229" height="868" alt="Screenshot From 2026-11-23 01-17-17" src="https://github.com/user-attachments/assets/3a0999a9-d252-4165-baa3-80d8f538b53c" />
<img width="1229" height="166" alt="Screenshot From 2026-11-23 01-16-36" src="https://github.com/user-attachments/assets/1c42c04b-8bde-4117-a182-be9b6996c52e" />
