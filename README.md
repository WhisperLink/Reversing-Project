# Reversing-Project

![Reverse Engineering](https://img.shields.io/badge/-Reverse%20Engineering-6f42c1?style=flat-square)
![PDF](https://img.shields.io/badge/-PDF-DC3545?style=flat-square)

Classic shareware crackme tutorials (Lena's Tutorials) used to practice reverse engineering fundamentals.

PDF walkthroughs included:
- Flash Producer
- GIF Movie Gear
- MediaChance
- TechScheduler
- XoftSpy

## Challenges

Each walkthrough targets a shareware "crackme" and demonstrates a different reverse engineering approach. The tools used are mainly OllyDbg and Resource Hacker.

### Flash Producer

A registration-bypass challenge. Rather than recovering a valid key, the goal is to patch the program so it behaves as registered. The window title switches between "Flash Jigsaw Producer" and "Flash Jigsaw Producer (unregistered)" via two `SetWindowTextA` calls, so the walkthrough searches for the "unregistered" string, locates the `TEST AL,AL` / conditional jump that decides registered state, and patches it to unlock the registered-only features.

### GIF Movie Gear

A nag-screen removal challenge. The trial shows a "This Trial Software expires in 30 days" dialog on startup. Using Resource Hacker the nag dialog is identified as resource ID 100 (0x64), then the code that calls `DialogBoxParam` with `push 64` is searched, breakpointed, and traced so the nag call can be removed/patched.

### MediaChance

A serial-validation challenge (Real-DRAW). The program asks for Name, Serial, and Unlock Code and shows "Please check Serial Number..." on failure. The walkthrough searches for the error strings, sets breakpoints on their references, and traces the validation routine — which includes a length check requiring the serial to be at least 8 characters — to understand and defeat the check.

### TechScheduler

A registration-bypass challenge. The registration key is validated as an integer, and entering an invalid key raises "not a valid integer value" followed by "Registration Key Failed!". The walkthrough uses string search to reach the compare and branch that chooses between "Registration Key accepted!" and "Registration Key Failed!", then patches the conditional jump to force acceptance.

### XoftSpy

A registration-bypass challenge solved through API analysis rather than string search. Using OllyDbg's "All intermodular calls" view, the `GetWindowTextA` / `GetWindowTextLength` calls that read the username and registration code are located and breakpointed (the input is read multiple times), and the validation reached from those calls is traced and patched to accept the code.
