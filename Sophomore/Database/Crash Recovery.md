# <center>Crash Recovery</center>

[TOC]

## Aries

Every page in the database contains the LSN called **pageLSN** of the most recent log record that describe a change to this page.

Five types of log record

- **Updating a page**
- **Commit**: The log tail is written to stable storage including the commit record
- **Abort**
- **End**: After all additional steps are completed for abort or commit log record, an *end* type log record containing the transaction id is appended to the log.
- **Undoing an update**

Two tables

- **Transaction table**
- **Dirty page table**

### Actions that need not be redone

- The affected page is not in the dirty page table, or
- The affected page is in the dirty page table, but the recLSN for the entry is greater than the LSN of the log record being checked, or
- The pageLSN (stored on the page, which must be retrieved to check this condition) is greater than or equal to the LSN of the log record being checked.

### Organize the ToUndo list

- If it is a CLR, and the undoNextLSN value is not null, the undoNextLSN value is added to the set ToUndo; if the undoNextLSN is null, an end record is written for the transaction because it is completely undone, and the CLR is discarded.
- If it is an update record, a CLR is written and the corresponding action is undone, as described in Section 20.1.1, and the prevLSN value in the update log record is added to the set ToUndo.

