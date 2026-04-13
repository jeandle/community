# Meeting Summary: 2026-4-9

## 📅 Meeting Information

Thank you for attending the Jeandle community meeting on March 26, 2026! Below is a summary of the main discussion points. If you have any suggestions for community improvement, please feel free to raise them [in an issue](https://github.com/jeandle/community/issues).

- **Date & Time**: April 9, 2026, 19:00 - 20:00
- **Location/Format**: Online Meeting
- **Host**: @Cruise20
- **Note Taker**: @taofengliu
- **Related Links**: https://github.com/jeandle/community/issues/44

## 👥 Attendees

- Community members, 13 participants

## 📋 Main Topics

### 🚀 Main Meeting Agenda
1. Introduction of the main progress during last weeks.
2. Next phase planning discussion.
3. Tech talk about On Stack Replacement(OSR): https://github.com/jeandle/document/blob/main/compilation/osr.md

### Q&A
1. Q: Why can't OSR compilation directly switch from C1 to C2?

   A: Currently, Hotspot handles the switch from C1 to C2 in OSR by deoptimizing the corresponding C1 OSR to interpreter execution when the C2 OSR compilation is complete. Then interpreter execution will switch to the C2 OSR immediately upon reaching the back-edge threshold. To allow a direct switch from C1 to C2, the execution state switching mechanism needs to be rewritten (deoptimization currently cannot switch from C1 to C2). Furthermore, the interpreter, which holds a stable and complete execution state, can perform a check of the virtual machine state during the C1 to C2 transition.

## 📎 Attachments & References

- PPT: [progress&plan.pptx](./progress&plan.pptx)
