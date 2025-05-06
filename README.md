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
- [x] Initial Architecture Specification
- [x] Increment Planning
- [ ] Increment 1
- [ ] Increment 2
- [ ] Increment 3
- [ ] Increment 4
- [ ] Increment 5

## User Requirements

01. Displays a spreadsheet-like visualization of an APL relation/table.
02. Data is stored efficiently for typical data workflows.
03. Spreadsheet can be scrolled around dynamically.
04. User can edit a cell by clicking on it.
05. Editing can support long contents.
06. User can save the sheet to a file.
07. User can load a sheet from a file previously saved.
08. User can import a sheet from a CSV.
09. Edits should auto-save
10. Sheets can be sorted by columns.
11. Sorting can ignore headers, optionally.
12. User can open a remotely shared file between 2 or more users.
13. User can live-share a sheet to a remote share server.
14. Must use an server-wide authentication token to share a sheet or access one.
15. Two people can edit different cells simultaneously.
16. Two people cannot edit the same cell simultaneously.
17. Remotely shared sheets disappear from the remote server when all users unlink.

## Architecture

There are two primary sub-systems:

* Sheets Application
* Document Server

Each syb-system is contained in a single namespace with `Run` entry point.

The sheets application uses Conga to communicate over a single websocket to an 
HTMLRenderer instance for its UI. We will use plain HTML for the GUI plus 
a JavaScript engine to render state changes in the system. 

Data is represented as a standard inverted table representation with column 
types and optional header names. 

Data is persisted by serializing to component files, with one component file 
per inverted table/sheet. 

Communication with the document server is accomplished by an always on 
binary conga socket. 

### Client UI Websocket protocol

The HTMLRenderer front-end UI interface communicates with the Dyalog APL session
via a single websocket connection. 

When an event occurs on the interface side, the interface will send an event 
to the session of the form:

	[event_type, data]

Where the `data` and the `event_type` sets are to be defined.

The session will send messages to the interface in the form of an array of
"rendering" messages, which take the form of:

	[node_id, command, data]

Where `node_id` is the node being manipulated, and the following set of 
commands and data pairs are possible:

| command | data                        |
| ------- | --------------------------- |
| `html`  | `"html_string"`             |
| `text`  | `"text_string"`             |
| `attr`  | `["attribute", "contents"]` |

Each of these updates the appropriate contents of the node identified by 
`node_id`. 

In the case of `html` commands, the `innerHTML` contents are updated with 
the data string.

In the case of `text` commands, the `innerText` property is updated.

In the case of `attr` commands, the `"attribute"` will be set with 
`setAttribute` to `"contents"`. 

### Document Server socket protocol

TBD

### Architectual background information

Much of this architectural design is based on the SAM pattern [2]. 

## Increments

These increments divide up the work into feedback cycles of implementation, 
certification, and evaluation. 

### Increment 1: Initial display

**Goal**: Get the basic architecture wiring complete, implement the core specification
loop, and get rendering and UI layout stuff to work. Basic minimal integration, 
without any core logic or interactivity. 

*Requirements*:

- [ ] R#01. Displays a spreadsheet-like visualization of an APL relation/table.
- [ ] R#02. Data is stored efficiently for typical data workflows.
- [ ] R#03. Spreadsheet can be scrolled around dynamically.

Technical Tasks:

- [x] Specify Client Update protocol
- [ ] Implement JS Render Engine
- [ ] Define UI Layout
- [ ] Define UI Style
- [ ] Specify sheet table encoding
- [ ] Specify core Sheets event loop
- [ ] Define core responses and stimuli
- [ ] Define minimal state
- [ ] Define Conga uplink
- [ ] Enable defining row and column size of table

### Increment 2: Cell Editing

**Goal**: To get core interactivity working and ensure that the JS rendering 
protocol is sufficient to the task.

*Requirements*:

- [ ] R#04. User can edit a cell by clicking on it.
- [ ] R#05. Editing can support long contents.

Technical Tasks:

- [ ] Define stimuli and responses for cell edits
- [ ] Update specification for handling cell edits
- [ ] Ensure state updates are consistent
- [ ] Define per cell updates in the UI rendering engine
- [ ] Define UI styling for actively edited cells

### Increment 3: Persistence

**Goal**: To enable local file persistence. We want to save and load files 
of the correct formats. 

*Requirements*:

- [ ] R#06. User can save the sheet to a file.
- [ ] R#07. User can load a sheet from a file previously saved.
- [ ] R#08. User can import a sheet from a CSV.
- [ ] R#09. Edits should auto-save

Technical Tasks:

- [ ] Define persistence functions
- [ ] Define stimuli and responses for loading and saving
- [ ] Define UI element for identifying where a file is saved
- [ ] Define CSV Import Format

### Increment 4: Sorting

**Goal**: To enable editable sorting

*Requirements*:

- [ ] R#10. Sheets can be sorted by columns.
- [ ] R#11. Sorting can ignore headers, optionally.

Technical Tasks:

- [ ] Enable a permutation vector on state
- [ ] Map permutation vector on edits
- [ ] Define reactive UI elements to sort columns
- [ ] Define sorting responses and stimuli

### Increment 5: Live sharing

**Goal**: To enable live sharing and editing

*Requirements*:

- [ ] R#12. User can open a remotely shared file between 2 or more users.
- [ ] R#13. User can live-share a sheet to a remote share server.
- [ ] R#14. Must use an server-wide authentication token to share a sheet or access one.
- [ ] R#15. Two people can edit different cells simultaneously.
- [ ] R#16. Two people cannot edit the same cell simultaneously.
- [ ] R#17. Remotely shared sheets disappear from the remote server when all users unlink.

Technical Tasks, Document Server:

- [ ] Define the document server protocol
- [ ] Specify and implement the document server stimuli and responses
- [ ] Define the document server specification
- [ ] Specify the document server state representation
- [ ] Define Conga uplink for the document server

Technical Tasks, Client application:

- [ ] Add stimuli and responses for document sharing
- [ ] Update specification to include document sharing events
- [ ] Add UI elements to handle sharing
- [ ] Specify remote sharing functions

## References

[1] https://www.amazon.com/Cleanroom-Software-Engineering-Technology-Process/dp/0201854805/

[2] https://sam.js.org/

[3] https://caseymuratori.com/blog_0001
