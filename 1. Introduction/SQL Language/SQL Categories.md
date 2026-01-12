SQL statements are typically organized into **five main categories** based on their function:

1.  **DDL (Data Definition Language)**
    *   **Purpose:** Defines and manages the structure of database objects (like tables, indexes, views).
    *   **Key Statements:** `CREATE`, `ALTER`, `DROP`, `TRUNCATE`, `RENAME`
    *   **Example:** `CREATE TABLE Employees (...);`

2.  **DML (Data Manipulation Language)**
    *   **Purpose:** Manipulates (inserts, updates, deletes) the actual data *within* those structures.
    *   **Key Statements:** `INSERT`, `UPDATE`, `DELETE`, `MERGE`
    *   **Example:** `UPDATE Employees SET salary = 75000 WHERE id = 101;`

3.  **DQL (Data Query Language)**
    *   **Purpose:** Used exclusively to retrieve data from the database. Often considered a subset of DML, but it's so fundamental it gets its own category.
    *   **Key Statement:** `SELECT`
    *   **Example:** `SELECT name, department FROM Employees;`

4.  **DCL (Data Control Language)**
    *   **Purpose:** Controls access to the data by managing user permissions and rights.
    *   **Key Statements:** `GRANT`, `REVOKE`
    *   **Example:** `GRANT SELECT ON Employees TO user_analyst;`

5.  **TCL (Transaction Control Language)**
    *   **Purpose:** Manages transactions, ensuring data integrity by grouping DML statements into logical units of work.
    *   **Key Statements:** `COMMIT`, `ROLLBACK`, `SAVEPOINT`
    *   **Example:** After a series of `INSERT` and `UPDATE` statements, you would issue a `COMMIT;` to finalize the changes.

---

### Focus on Table-Related Operations

Based on your mention of **"in tables,"** here's how these categories apply directly to **table operations**:

| Category | For Tables...                                                                 | Common Statements                     |
| :------- | :---------------------------------------------------------------------------- | :------------------------------------ |
| **DDL**  | **Defines the table's structure:** its columns, data types, keys, and constraints. | `CREATE TABLE`, `ALTER TABLE`, `DROP TABLE` |
| **DML**  | **Modifies the data stored inside the table rows.**                           | `INSERT INTO`, `UPDATE`, `DELETE FROM` |
| **DQL**  | **Queries/reads the data from one or more tables.**                           | `SELECT ... FROM ...`                 |
| **DCL**  | **Controls who can perform DDL/DML/DQL on the table.**                        | `GRANT` on the specific table         |
| **TCL**  | **Ensures a group of changes to table data are saved or undone together.**    | `COMMIT`, `ROLLBACK`                  |

### Quick-Reference Table of Key SQL Statements

| Category | Statement  | Purpose (with Table Focus)                                   |
| :------- | :--------- | :----------------------------------------------------------- |
| **DDL**  | `CREATE`   | Creates a new table.                                         |
|          | `ALTER`    | Modifies an existing table's structure (add/drop column).    |
|          | `DROP`     | Deletes an entire table and its data.                        |
|          | `TRUNCATE` | Removes all data from a table, keeping only the structure.   |
| **DML**  | `INSERT`   | Adds new rows of data to a table.                            |
|          | `UPDATE`   | Modifies existing data in specific table rows.               |
|          | `DELETE`   | Removes specific rows from a table.                          |
| **DQL**  | `SELECT`   | Retrieves data from one or more tables.                      |
| **DCL**  | `GRANT`    | Gives a user permission to access/modify a table.            |
|          | `REVOKE`   | Takes away a user's permission on a table.                   |
| **TCL**  | `COMMIT`   | Saves all changes made in the current transaction.           |
|          | `ROLLBACK` | Undoes all changes made in the current transaction.          |

In summary: **DDL** changes the *blueprint* of the table, **DML** changes the *contents* of the table, and **DQL** reads those contents. **DCL** and **TCL** are the governance and safety mechanisms around these actions.
