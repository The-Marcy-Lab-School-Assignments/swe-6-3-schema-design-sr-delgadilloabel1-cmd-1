# Short Response: Schema Design and Normalization

Answer each question below. Write in complete sentences (3–5 per answer).

---

## Question 1

The table below stores data for a library's checkout system. Identify every normalization rule it violates and describe how you would fix the schema. You do not need to write SQL — describe the tables you would create and why.

| checkout_id | patron_id | patron_name | patron_email     | book_id | book_title                | author_name       | genres                   |
| ----------- | --------- | ----------- | ---------------- | ------- | ------------------------- | ----------------- | ------------------------ |
| 1           | 10        | Maya Patel  | maya@email.com   | 201     | The Left Hand of Darkness | Ursula K. Le Guin | Science Fiction, Fantasy |
| 2           | 11        | Jordan Kim  | jordan@email.com | 202     | Beloved                   | Toni Morrison     | Fiction, Historical      |
| 3           | 10        | Maya Patel  | maya@email.com   | 202     | Beloved                   | Toni Morrison     | Fiction, Historical      |
| 4           | 12        | Sam Torres  | sam@email.com    | 201     | The Left Hand of Darkness | Ursula K. Le Guin | Science Fiction, Fantasy |

**Your answer:**

The table violates multiple normalization rules. It breaks because the `genres` column contains multiple values in a single field instead of atomic values. It also violates because patron and book details (like `patron_name`, `book_title`, and `author_name`) are repeated and depend on non-primary attributes rather than the checkout itself, creating redundancy. To fix this, I would create separate tables for `patrons`, `books`, and `authors`, and a `checkouts` table that links patrons to books. Additionally, I would create a separate `genres` table and a bridge table to handle the many-to-many relationship between books and genres.
---

## Question 2

Explain the difference between one-to-many relationships and many-to-many relationships by providing a real world example of each. Then describe how each is represented in a relational database. Use the term "association/bridge" table in your response.

**Your answer:**

A one-to-many relationship occurs when one record in a table is related to many records in another table, such as one author having many books. In a relational database, this is represented by placing a foreign key in the “many” table that references the primary key of the “one” table. A many-to-many relationship occurs when multiple records in one table relate to multiple records in another, such as students enrolling in multiple courses and courses having multiple students. This type of relationship cannot be represented directly, so it requires an association (bridge) table that contains foreign keys referencing both tables. The bridge table allows the database to efficiently track and enforce these relationships without redundancy.
---

## Question 3

What is referential integrity? How does PostgreSQL enforce it, and why does this enforcement determine the order in which you must create — and drop — tables?

**Your answer:**

Referential integrity ensures that relationships between tables remain consistent, meaning that foreign key values must match existing primary key values in the referenced table. PostgreSQL enforces this using foreign key constraints, which prevent inserting invalid references or deleting records that are still being used elsewhere. Because of this enforcement, tables must be created in a specific order: parent tables must exist before child tables that reference them. Similarly, when dropping tables, child tables must be removed first to avoid violating foreign key constraints. This ensures the database always maintains valid and consistent relationships between its data.

---

## Question 4

Why does an association table need a `UNIQUE (col1, col2)` constraint on its two foreign key columns? What specific problem does this prevent, and why wouldn't making each column individually `UNIQUE` solve it?

**Your answer:**

An association table needs a `UNIQUE (col1, col2)` constraint to prevent duplicate relationships between the same two records, such as a user being registered for the same event multiple times. This ensures that each pair of foreign keys appears only once, maintaining data integrity. If each column were individually marked as `UNIQUE`, it would incorrectly restrict each value to appear only once in the entire table, which would break valid relationships. The combined unique constraint allows each value to appear multiple times independently, but not as the same pair. This is essential for modeling many-to-many relationships without duplication.

---
