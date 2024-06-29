# Lesson 5 - Database Review

* What is a database server?
    * A collection of databases, permissions, and services to access the data safely.
* What are attributes of a database server that make it unique?
    * It handles database, table, and row level security.
    * It allows multiple users to access the same data effeciently and safely.
    * It allows multiple users to edit data effectively.
    * A relational database guarantees that any work in a transaction WILL be written to the database (disk). 
    * Most relational database are ACID complient (Atomic, Consistency, Isolation, Durability) - [Wikipedia Entry](https://en.wikipedia.org/wiki/ACID)
    * Most support additional procedural languages allowing data logic to be written in the database.
* What are the reasons we use database servers instead of files like excel or csv?
    * In order to use files effectively, you'd need to reproduce the multi user capabilities stated above.
    * File format are easily corrupted.
 * What's the difference between RDBMS and NoSql servers?
    * Emmediate conistency vs eventual consistency
    * Well defined schemas with related tables and constraints vs. wide rows where relationships are captured in the same row of the data.
    * Enforced constraints in RDBMS slow down inserts and reads as a cost of consistency.

### Terms
* Transaction - Is a logical unit of work that groups multiple operations into a single unit within the database.
* Pesimistic lock - is an RDBMS function that prohibits two threads from writing the same row at the same time.
* Optimistic lock - Is a technique for adding a revision number to a table and verify revisions prior to insert.
* Dead Lock - When two or more transactions are unable to proceed because they are indefinitly waiting for the other to complete.


## Strech Goals

* Write a procedure in the database that returns a **tree print out** from the employee table based on the self refrence of "reports_to" and share your output in Discord. (DO NOT SHARE THE SOURCE, ONLY THE OUTPUT)

