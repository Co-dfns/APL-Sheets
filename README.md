# APL-Sheets

A sample Spreadsheet style program in APL. 

The following sections are a sort of "Project Record" for the development of this
application, to give the reader context in the development process of making 
this application as well as guidance on how other such applications can be 
developed as well. These sections are designed to be augmented by the commit 
log of the repository. 

Much of this overall process is an adaptation of Cleanroom Software Engineering
to a particular view of APL; see [1] for more information.

## The Design Principles

1. APL is an executable specification language
2. Try to implement your system as a specification
3. Make the system "provable/verifiable" first and foremost
4. Emphasize stochastic, black-box testing to certify system behavior
5. Favor linear data flow architectures
6. Minimize entanglement, dependencies, and abstraction
7. Keep state global, isolated, sequential, and potentially serialized
8. Utilize mathematical APL for as much as is easily verifiable using such methods
9. Utilize sequence-based function specification for defining inherently event-driven, branchy code
10. Utilize the immediate-mode GUI paradigm [3] for user-interface interactions

## Project Phases

- [x] Project Plan
- [x] User Requirements Analysis
- [ ] Initial Architecture Specification
- [ ] Increment Planning
- [ ] Increment 1
- [ ] ...

## User Requirements

1. Displays a spreadsheet-like visualization of an APL relation/table.
2. Data is stored efficiently for typical data workflows.
3. Spreadsheet can be scrolled around dynamically.
4. User can save the sheet to a file.
5. User can load a sheet from a file previously saved.
6. User can import a sheet from a CSV.
7. User can edit a cell by clicking on it.
8. Editing can support long contents.
9. Sheets can be sorted by columns.
10. Sorting can ignore headers, optionally.
11. User can open a remotely shared file between 2 or more users.
12. User can live-share a sheet to a remote share server.
13. Must use an authentication token to share a sheet or access one.
14. Two people can edit different cells simultaneously.
15. Two people cannot edit the same cell simultaneously.
16. Remotely shared sheets disappear from the remote server when all users unlink.

## References

[1] https://www.amazon.com/Cleanroom-Software-Engineering-Technology-Process/dp/0201854805/
[2] https://sam.js.org/
[3] https://caseymuratori.com/blog_0001
