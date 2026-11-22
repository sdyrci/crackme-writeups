Disassembling main reveals that the program sums two hardcoded constants (0x5a + 0x1ec = 582), squares the result (582 * 582 = 338724), and passes both the user input (EDI) and the computed target (ESI) to the test function. Setting a breakpoint at the cmp instruction inside test (*test+21) and inspecting $rbp-0x8 confirms the expected password is 338724. Passing this value bypasses the jne check and triggers shift, which subtracts 3 from each ASCII character to decode the success message.


<img width="1187" height="629" alt="Screenshot From 2026-11-23 00-12-33" src="https://github.com/user-attachments/assets/67798676-84aa-46f4-aebc-7e32784eb1c7" />
<img width="1437" height="447" alt="Screenshot From 2026-11-23 00-16-25" src="https://github.com/user-attachments/assets/939062e3-b35a-4dbe-b552-7914eee20625" />
<img width="1187" height="47" alt="Screenshot From 2026-11-23 00-16-09" src="https://github.com/user-attachments/assets/5c784902-6c06-4613-a8e1-69bde67624c8" />
<img width="1437" height="149" alt="Screenshot From 2026-11-23 00-16-57" src="https://github.com/user-attachments/assets/dc56fbd7-563f-46bb-b57a-887c81e4de6f" />
