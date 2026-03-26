# Meeting Summary: 2026-3-26

## 📅 Meeting Information

Thank you for attending the Jeandle community meeting on March 26, 2026! Below is a summary of the main discussion points. If you have any suggestions for community improvement, please feel free to raise them [in an issue](https://github.com/jeandle/community/issues).

- **Date & Time**: March 26, 2026, 19:00 - 20:00
- **Location/Format**: Online Meeting
- **Host**: @taofengliu
- **Note Taker**: @Cruise20
- **Related Links**: https://github.com/jeandle/community/issues/40

## 👥 Attendees

- Community members, 8 participants

## 📋 Main Topics

### 🚀 Main Meeting Agenda
1. Introduction of the main progress during last weeks.
2. Next phase planning discussion.
3. Jeandle roadmap in 2026 discussion.

### Q&A
1. Q: How much work would it take to implement the inlining algorithm described in the paper in the C2 compiler?
   A: It might require quite a lot of work, because it’s very different from how C2 currently operates. On top of that, C2’s code is notoriously hard to modify, so the risk involved in changing this part also needs to be taken into account.
2. Q: You just mentioned that LLVM cannot represent Java types. Then how does LLVM handle C++ classes, and why can’t Jeandle use the same approach?
   A: LLVM uses struct types to represent C++ classes, but that also fixes their memory layout. We can’t map that memory layout directly to Java objects.

## 📎 Attachments & References

- PPT: [progress&plan.pptx](./progress&plan.pptx)
