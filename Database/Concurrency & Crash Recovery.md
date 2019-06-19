# <center>Concurrency & Crash Recovery</center>

[TOC]

## 2PL, Strict 2PL, Serializability and Recoverability

### Anomalies associated with synchronization

#### Reading Uncommitted Data (WR Conflicts)

<div align="center">
    <img src="Pic/Reading uncommitted data.PNG">
</div>

#### Unrepeatable Reads (RW Conflicts)

Read before write, and two problems may arise

- If the reading transaction tries to read the value again, it will get a different result.
- If the reading transaction writes latter, this write will invalidate the assumption about the value read, which may be the basis for the reading transaction's subsequent actions.

#### Overwriting Uncommitted Data (WW Conflicts)

### Conflict equivalent

- Involve the (same set of) actions of the same transactions and order every pair of conflicting actions of two committed transactions in the same way.
- A schedule is **conflict serializable** if and only if its precedence graph is acyclic or it's conflict equivalent to some serial schedules.

### Strict 2PL & 2PL

- Strict 2PL

  - If a transaction T wants to read (respectively, modify) an object, it first requests a shared (respectively, exclusive) lock on the object.
  - All locks held by a transaction are released when the transaction is completed.

- 2PL

  - Same with Strict 2PL

  - A transaction cannot request additional locks once it releases any lock, which means transactions can release locks before the end.

  - Accept

    <div align="center">
        <img src="Pic/Acceptance of 2PL.png">
    </div>

   Both Ensures the precedence graph for any schedule that they allows is acyclic.

- A **strict** schedule means a value written by a transaction T is not read or overwritten by other transactions until T either aborts or commits. Strict schedules are **recoverable, do not require cascading aborts**. Strict 2PL improves on 2PL by guaranteeing that every allowed schedule is strict.

### View equivalent

- If $T_i$ reads the initial value of object A in $S_1$, it must also read the initial value of A in $S_2$.

- If $T_i$ reads a value of A written by $T_j$ in $S_1$, it must also read the value of A written by $T_j$ in $S_2$.

- For each data object A, the transaction (if any) that performs the final write on A in $S_1$ must also perform the final write on A in $S_2$.

- Accept

  <div align="center">
      <img src="Pic/Acceptance of view serializable.png">
  </div>

- **Hint**: any view serializable schedule that is not conflict serializable contains a blind write.

## Lock Management

### Deadlock

#### Deadlock prevention

Give each transaction a priority and ensure that lower priority transactions are not allowed to wait for higher priority transactions. For example, give each transaction a **timestamp** when it starts up, and the lower the timestamp, the higher the transaction's priority. In this case, the older transactions have higher priority.

Two policies to deal with the situation when a transaction $T_i$ requests a lock held by $T_j$

- **Wait-die**: If $T_i$ has higher priority, it's allowed to wait; otherwise it's aborted.
- **Wound-wait**: If $T_i$ has higher priority, abort $T_j$; otherwise $T_i$ waits.
- In these two schemes, to ensure the equality that no transaction is perennially aborted because it never has a sufficiently high priority, it should be given the same timestamp that it had originally when a transaction is aborted and restarted.

#### Deadlock detection

Maintain a waits-for graph to detect deadlock cycles.

<div align="center">
    <img src="Pic/Schedule for deadlock detection.PNG">
</div>

<div align="center">
    <img src="Pic/Waits-for.png">
</div>

### Implementation

#### Dynamic databases and phantom

Ways to handle the problem

- If there is no index, and all pages in the file must be scanned, lock-holder must somehow
  ensure that no new pages are added to the file, in addition to locking all existing pages.
- Index locking: If there is a dense index on the rating field, lock-holder can obtain a lock on the index page. The pages that contain related data entries are locked, and any transaction that tries to insert a record must insert a data entry pointing to the new record into the index page and is blocked until the lock on index has been released.

#### Concurrency control in B+ trees

If the leaf is full and we have to upgrade its parent node from shared lock to exclusive lock, which may leads to deadlock, if a transaction holding a shared lock on the parent waits for the leaf.

#### Multiple-Granularity Locking

**Lock escalation**: First obtain fine granularity locks and after the transaction requests a certain number of locks at that granularity, start obtaining locks at the next higher granularity.

## Transaction support in SQL

### Isolation level

- SERIALIZABLE: Agree with strict 2PL, and have index locking to prevent from phantom.
- REPEATABLE READ: Agree with strict 2PL but do not have index locking.
- READ COMMITTED: Hold exclusive locks to the end for writing but release shared locks for reading immediately only to guarantee that the transaction that last modified the object is complete.
- READ UNCOMMITTED: Do not obtain shared locks before reading objects.

Ways to update the current transaction's isolation level

SET TRANSACTION ISOLATION LEVEL SERIALIZABLE READ ONLY

### Integrity constraints

#### Constraints over a single table

<div>
    <img src="Pic/Constraints over tables.PNG">
</div>

#### Domain constraints

<div align="center">
    <img src="Pic/Domain constraints.png">
</div>

#### Assertions

<div align="center">
    <img src="Pic/Assertions.png">
</div>

Constraint checking can be in DEFERRED or IMMEDIATE mode.

SET CONSTRAINT ConstraintFoo DEFERRED

A constraint that is in deferred mode is checked at commit time.

#### Triggers

<div>
    <img src="Pic/Triggers.png">
</div>

The Trigger init_count is a statement-level trigger which only runs just once per INSERT statement and incr_count occurs for each modified record for "FOR EACH ROW".

<div align="center">
    <img src="Pic/Instead of.png">
</div>
## Concurrency without locking

### Procedure

- **Read**: The transaction executes, reading values from the database and writing
  to a private workspace
- **Validation**: If the transaction decides that it wants to commit, the DBMS checks
  whether the transaction could possibly have conflicted with any other concurrently
  executing transaction. If there is a possible conflict, the transaction is aborted;
  its private workspace is cleared and it is restarted.
- **Write**: If validation determines that there are no possible conflicts, the changes
  to data objects made by the transaction in its private workspace are copied into
  the database.

#### Details of validation

For each pair of transactions $T_i$ and $T_j$ such that $TS(T_i) < TS(T_j)$, one of the following conditions must hold.

- $T_i$ completes (all three phases) before $T_j$ begins; or
- $T_i$ completes before $T_j$ starts its Write phase, and $T_i$ does not write any database
  object that is read by $T_j$; or
- $T_i$ completes its Read phase before $T_j$ completes its Read phase, and $T_i$ does not
  write any database object that is either read or written by $T_j$.

## Security

```
Grant [privileges including(insert, delete, update)] on Reserves to Yuppy (with grant option, if have then can pass the privileges to others.)
Revoke [grant option for] privileges on object from users {Restrict | Cascade}
```

