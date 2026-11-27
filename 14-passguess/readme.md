Disassembling the main function in pwndbg revealed that the user input captured via scanf is compared against a hardcoded string in memory using strcmp at main+110 (0x11e7). During dynamic analysis, setting a breakpoint directly on this comparison call (b *main+110) paused execution right before the check. Inspecting the memory address pointed to by the $rsi register—which holds the second argument passed to strcmp—with x/s $rsi exposed the plaintext password "7Wtyr", successfully bypassing the authentication check when provided as input.

<img width="1299" height="851" alt="Screenshot From 2026-11-27 01-04-52" src="https://github.com/user-attachments/assets/d0417566-575f-48d0-a25e-cc6272646adb" />
<img width="1299" height="71" alt="Screenshot From 2026-11-27 01-05-47" src="https://github.com/user-attachments/assets/a3ccf2b5-61c0-4212-bef1-06e4a38bf392" />
<img width="1299" height="96" alt="Screenshot From 2026-11-27 01-06-34" src="https://github.com/user-attachments/assets/6dce0ba1-5bb1-4c1e-9cc6-80fd362ff6bb" />
<img width="1303" height="128" alt="Screenshot From 2026-11-27 01-07-10" src="https://github.com/user-attachments/assets/139a21fd-f33b-4992-9b45-4013c5b72e16" />
