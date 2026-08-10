# The Complete DBMS Guide
### From Syllabus Fundamentals to Interview-Ready Depth

This document covers your full 5-unit DBMS syllabus in detail, plus twelve additional topic areas (indexing, query optimization, advanced SQL, transaction internals, recovery, advanced concurrency control, storage, distributed databases, data warehousing, deeper NoSQL, security, and practical constraints/views/triggers) that are commonly tested in interviews and placements but often skipped in coursework.

**How to use this**: Read it in order the first time. After that, use it as a reference — the headers are structured so you can jump straight to what you need.

---

## Table of Contents

**Part A — Core Syllabus**
1. [Introduction to Database Systems](#1-introduction-to-database-systems)
2. [Entity-Relationship (ER) Model](#2-entity-relationship-er-model)
3. [ER-to-Relational Mapping & ER Diagram Design](#3-er-to-relational-mapping--er-diagram-design)
4. [Relational Model & Relational Algebra](#4-relational-model--relational-algebra)
5. [SQL](#5-sql)
6. [Relational Database Design — Functional Dependencies & Normalization](#6-relational-database-design--functional-dependencies--normalization)
7. [Transaction Processing Concepts](#7-transaction-processing-concepts)
8. [Concurrency Control (Basic)](#8-concurrency-control-basic)
9. [NoSQL & Big Data](#9-nosql--big-data)

**Part B — Beyond the Syllabus (Interview-Critical)**
10. [Indexing & File Organization](#10-indexing--file-organization)
11. [Query Processing & Optimization](#11-query-processing--optimization)
12. [Advanced SQL](#12-advanced-sql)
13. [Transactions — Advanced Depth](#13-transactions--advanced-depth)
14. [Database Recovery Techniques](#14-database-recovery-techniques)
15. [Concurrency Control — Advanced](#15-concurrency-control--advanced)
16. [Storage & Disk Management](#16-storage--disk-management)
17. [Distributed Databases](#17-distributed-databases)
18. [Data Warehousing & OLAP](#18-data-warehousing--olap)
19. [NoSQL — Deeper Dive](#19-nosql--deeper-dive)
20. [Security & Authorization](#20-security--authorization)
21. [Triggers, Views & Constraints — Practical Depth](#21-triggers-views--constraints--practical-depth)

---

# 1. Introduction to Database Systems

## 1.1 What is a Database, and why not just use files?

A **database** is a structured, self-describing collection of related data, designed and managed so many applications and users can share it. A **DBMS (Database Management System)** is the software layer that lets you define, construct, manipulate, and share this data safely.

Before DBMSs, systems used **file processing**: each application had its own files (e.g., `payroll.dat`, `students.dat`), with the application code hard-wired to the file's layout. This caused four classic problems, which is the "textbook" motivation you'll be asked about:

| Problem | What it means | DBMS fix |
|---|---|---|
| **Redundancy & inconsistency** | Same data (e.g., a student's address) stored in multiple files, updated in one but not another | Centralized data, single source of truth |
| **Difficulty accessing data** | Need a new program for every new kind of query | Declarative query language (SQL) — ask *what*, not *how* |
| **Data isolation** | Data scattered across files/formats, hard to write programs that pull from several | Unified schema, shared storage |
| **Integrity problems** | Constraints (e.g., "balance ≥ 0") buried in scattered application code, easy to miss in one program | Constraints declared once, centrally enforced |
| **Atomicity / concurrency / security problems** | Crashes leave data half-updated; concurrent access corrupts data; no fine-grained access control | Transactions (ACID), concurrency control, authorization |

## 1.2 Characteristics of the Database Approach

This is a favorite direct-definition exam question — know these five cold:

1. **Self-describing nature** — A database system contains not just data, but a description of the data itself (the **metadata**, stored in the **catalog** or **data dictionary**). This is what lets the DBMS be general-purpose software rather than code written for one specific dataset.
2. **Insulation between programs and data / data abstraction** — The structure of data files is stored in the catalog separately from the access programs. This is called **program-data independence**. Change the internal storage structure and, ideally, application code doesn't break.
3. **Support for multiple views of the data** — Different users can see different, tailored representations of the same underlying data (e.g., a payroll clerk's view vs. an HR manager's view).
4. **Sharing of data and multi-user transaction processing** — Many users can access the same database concurrently, ideally without stepping on each other's toes. This requires **concurrency control**.
5. Additional properties databases typically provide: control of redundancy, restricting unauthorized access, provision for persistent storage, providing backup/recovery, providing multiple interfaces, representing complex relationships, enforcing integrity constraints, and drawing inferences via rules.

## 1.3 Database Users

Broadly split into two groups:

**Actors on the scene** (people who use/manage data day to day):
- **Database Administrators (DBA)**: own the central resource — the database itself. Authorize access, coordinate/monitor use, acquire resources, approve schema/storage changes.
- **Database Designers**: identify the data to be stored, choose structures to represent and store it — usually before the database exists in production.
- **End users**: the people the database exists for. Subtypes:
  - *Casual end users*: occasionally access via a query language (e.g., a manager querying sales data once a month).
  - *Naive/parametric end users*: constantly query/update via canned transactions (e.g., a bank teller, always running the same few predefined operations).
  - *Sophisticated end users*: engineers, scientists, analysts who thoroughly understand the system and write their own applications/complex queries.
  - *Standalone users*: maintain personal databases using off-the-shelf packages.
- **System analysts and application programmers (software engineers)**: analysts determine end-user requirements; programmers implement these as application programs.

**Workers behind the scene** (build/maintain the DBMS itself, not "users" of a specific database): DBMS system designers/implementers (build the DBMS software), tool developers, operators, maintenance staff.

## 1.4 Data Models, Schemas, and Instances

A **data model** is a collection of concepts for describing the structure of a database — the data types, relationships, and constraints. Data models are grouped by abstraction level:

- **High-level / conceptual data models**: close to how users perceive data — entities, attributes, relationships. The ER model lives here.
- **Low-level / physical data models**: describe how data is stored — record formats, orderings, access paths (indexes).
- **Representational (implementation) data models**: in between — used by actual DBMS software. The **relational model** is the dominant example. (Older ones: network model, hierarchical model.)
- **Self-describing data models** (a newer category): combine data description with the data values themselves — e.g., key-value stores, XML, JSON-based NoSQL models.

### Schema vs. Instance — this distinction is asked constantly

- A **schema** is the *description* of a database — the structure, data types, and constraints. Specified at database design time and doesn't change often. Think of it like a variable's *type declaration* in programming.
- A **database instance** (or **state**, or **snapshot**) is the *actual data* stored in the database at a particular moment. Changes every time you insert/delete/update. Think of it like the variable's *value* at runtime.
- Every time the database is updated, it moves from one valid state to another. The DBMS is partly responsible for ensuring every state satisfies the schema's structure and constraints.

### Diagrammatic Notation

A **schema diagram** displays schema constructs (not data). Each object in the diagram is a **schema construct** (e.g., `STUDENT`, `COURSE`).

## 1.5 The Three-Schema Architecture and Data Independence

Proposed by ANSI/SPARC, this is *the* central architectural concept of the unit and comes up in nearly every DBMS exam:

```
┌─────────────────────────────────────────┐
│         EXTERNAL LEVEL                    │   Multiple "user views" —
│   View 1     View 2     View 3            │   each tailored to a
│                                            │   group of end users
└───────────────────┬───────────────────────┘
                     │  external/conceptual mapping
┌────────────────────▼───────────────────────┐
│         CONCEPTUAL LEVEL                    │   One single conceptual
│   Entire database structure for the         │   schema — describes ALL
│   whole community of users, hides           │   entities, relationships,
│   physical storage detail                   │   constraints, no storage
│                                              │   detail
└────────────────────┬─────────────────────────┘
                     │  conceptual/internal mapping
┌────────────────────▼───────────────────────┐
│         INTERNAL LEVEL                      │   Physical storage
│   Storage structures, file organizations,   │   description: indexes,
│   indexes, access paths                     │   compression, record
│                                              │   placement
└──────────────────────────────────────────────┘
```

- **Internal level**: the physical storage description — how data is actually stored (files, indexes, compression). Uses a **physical/internal schema**.
- **Conceptual level**: describes the structure of the *whole* database for the entire community of users — entities, data types, relationships, constraints — but hides physical storage details. Uses a **conceptual schema**.
- **External (view) level**: describes the part of the database a particular user group is interested in, hiding the rest. Uses **external schemas / user views**.

Note: these are three *descriptions* of data, not three copies — only physical-level data actually exists in storage.

### Data Independence — the payoff of the 3-schema architecture

- **Logical data independence**: the capacity to change the *conceptual* schema without having to change *external* schemas or application programs. (e.g., adding a new entity/attribute shouldn't break existing views, though programs that reference *removed* things will break.)
- **Physical data independence**: the capacity to change the *internal* schema without changing the conceptual (or external) schema. (e.g., reorganizing files, creating a new index, switching storage devices — application programs are unaffected.)

Physical data independence is easier to achieve than logical data independence, because applications tend to be more functionally tied to the logical structure of the data than to its physical storage details.

**Mappings**: The DBMS must transform a request specified at the external level into one at the conceptual level, and then into one at the internal level for processing over the stored database. If the schemas at any level change, mapping between that level and the level above it must be updated — but the schemas at the other levels can stay the same. This is exactly *how* data independence is achieved.

## 1.6 The Database System Environment

A full DBMS deployment involves multiple software components:

- **DDL compiler**: processes schema definitions (written in the Data Definition Language) and stores their description (metadata) in the DBMS **catalog**.
- **Catalog / data dictionary**: stores metadata — descriptions of the schema constructs and constraints. Queried by the DBMS during processing.
- **Runtime database processor**: handles database access at runtime, executing the retrieval/update requests. Works with the catalog and the compiled/interpreted queries.
- **Query compiler / optimizer**: parses and optimizes high-level (SQL) queries into something the runtime processor can execute efficiently.
- **Precompiler**: extracts DML commands embedded in a host programming language for separate compilation by the DML compiler.
- **Stored data manager**: controls access to actual DBMS data (on disk), often working closely with the OS's file system.

Two-tier vs. three-tier client-server architecture is worth knowing at a high level: in **two-tier**, the client directly talks to the DBMS server; in **three-tier** (common on the web), a middle **application/web server** sits between thin clients and the database server, handling business logic — improving scalability, security, and separation of concerns.

---

# 2. Entity-Relationship (ER) Model

The ER model is a **high-level conceptual data model**, used for database *design* — a way to think about and communicate a database's structure before committing to any specific DBMS. It is deliberately independent of implementation details.

## 2.1 Entity Types, Entity Sets, Attributes, and Keys

- **Entity**: a "thing" in the real world with an independent existence — a specific person, company, event, or object (e.g., the employee "John Smith").
- **Entity type**: defines a *collection* (class) of entities that have the same attributes (e.g., `EMPLOYEE`).
- **Entity set**: the collection of all entities of a particular entity type in the database at a given moment (this is the *extension* of the entity type — mirrors the schema/instance distinction from Section 1.4).

### Attributes

Every entity has attributes — properties that describe it. Types of attributes (a very common short-answer question — know each with an example):

| Attribute type | Meaning | Example |
|---|---|---|
| **Simple (atomic)** | Not divisible further | `Age` |
| **Composite** | Can be divided into smaller sub-parts | `Address` → `Street`, `City`, `State`, `Zip` |
| **Single-valued** | Has only one value for a given entity | `SSN` |
| **Multivalued** | Can have multiple values (shown with `{}`) | `{PhoneNumbers}` — a person can have several |
| **Stored** | Directly stored, not derivable | `Date_of_birth` |
| **Derived** | Computed from another attribute | `Age` (derived from `Date_of_birth`), shown with a dashed oval |
| **Complex** | Composite AND multivalued combined | `{Address(Street, City)}` for someone with multiple addresses |
| **NULL-valued** | Value unknown, not applicable, or unavailable | A missing `Apartment_Number` |

### Keys

- **Key (candidate key) attribute**: an attribute (or set of attributes) whose value is distinct for every individual entity in the entity set. An entity type may have *more than one* key.
  - Example: `EMPLOYEE` might have both `SSN` and `Employee_ID` as candidate keys.
- **Composite key**: a key made up of multiple attributes together (no single one of which is unique alone).
- When an entity type has more than one key, one is chosen as the **primary key** at design time; the rest become **candidate keys** (sometimes just called "unique" in implementation).
- Some entity types have **no key** — these are always **weak entity types** (see 2.3).

### Value Sets (Domains)

Each simple attribute is associated with a **value set (domain)** — the set of legal values it can take (e.g., `Age`'s domain might be integers 0–150).

## 2.2 Relationship Types, Sets, Roles, and Structural Constraints

- **Relationship**: an association between two or more entities (e.g., "employee *works for* department").
- **Relationship type**: defines a set of associations — a relationship among entity types.
- **Relationship set**: the current set of relationship instances of a relationship type (again mirrors schema/instance).
- **Degree of a relationship type**: number of participating entity types.
  - **Binary** (degree 2) — by far the most common: `WORKS_FOR(EMPLOYEE, DEPARTMENT)`
  - **Ternary** (degree 3): e.g., `SUPPLY(SUPPLIER, PART, PROJECT)` — a supplier supplies a part to a project. Important: a ternary relationship is generally **not** equivalent to three binary relationships — it can capture constraints the binary decomposition loses.
  - **Recursive relationship**: same entity type participates more than once, in different roles — e.g., `EMPLOYEE` `SUPERVISES` `EMPLOYEE` (a manager supervises other employees, who are also `EMPLOYEE`s). Here, **roles** matter: the same entity type plays a `Supervisor` role and a `Supervisee` role.

### Role names

A **role name** identifies the function an entity plays in a relationship. Essential in recursive relationships to distinguish participation (e.g., `Supervisor` vs. `Supervisee`), and generally useful for clarity even in non-recursive relationships involving the same entity type twice.

### Structural Constraints — cardinality ratio and participation

Two kinds of constraints on binary relationship types — **these are the single most tested part of ER theory**, so let's be precise:

**1. Cardinality ratio** — how many relationship *instances* an entity can participate in.
For a binary relationship type R between entity types E1 and E2:

| Ratio | Meaning | Example |
|---|---|---|
| **1:1** | One entity of E1 relates to at most one of E2, and vice versa | One `EMPLOYEE` manages one `DEPARTMENT`; one `DEPARTMENT` has one manager |
| **1:N** | One entity of E1 relates to many of E2, but each E2 entity relates to at most one E1 | One `DEPARTMENT` has many `EMPLOYEE`s; each `EMPLOYEE` works for one `DEPARTMENT` |
| **N:1** | Mirror image of 1:N — just depends on which side you name first | |
| **M:N** | Entities on both sides can relate to many on the other side | An `EMPLOYEE` can work on many `PROJECT`s; a `PROJECT` can have many `EMPLOYEE`s |

**2. Participation constraint** — whether *every* entity of a type must participate in the relationship, or only *some*.

- **Total participation** (existence dependency): every entity in the entity set *must* participate in at least one relationship instance. Drawn as a **double line** in ER diagrams.
  - Example: every `EMPLOYEE` must work for some `DEPARTMENT` → total participation of `EMPLOYEE` in `WORKS_FOR`.
- **Partial participation**: only *some* entities participate. Drawn as a **single line**.
  - Example: not every `EMPLOYEE` manages a department → partial participation of `EMPLOYEE` in `MANAGES`.

Together, cardinality ratio + participation constraint are often summarized with **(min, max)** notation, e.g., `EMPLOYEE` participates in `WORKS_FOR` with (1,1) — must relate to exactly one department — while `DEPARTMENT` participates with (0,*) — can have zero or many employees at a point in time, though realistically at least one.

### Attributes of relationship types

A relationship type itself can have attributes — e.g., `WORKS_ON(EMPLOYEE, PROJECT)` might have an attribute `Hours` (how many hours that employee works on that project). This attribute belongs to the *relationship*, not to either entity, because its value depends on the *combination* of both.

## 2.3 Weak Entity Types

- A **weak entity type** does not have its own key attribute — it has no attributes that are unique across all its entities.
- It must be **existence-dependent** on another entity type, called its **identifying** (or **owner**) entity type, via a special relationship called an **identifying relationship**. The weak entity always has **total participation** in this relationship.
- The weak entity type has a **partial key** (also called a **discriminator**): an attribute that can uniquely identify weak entities *that belong to the same owner entity* (but not uniquely across the whole database).
- Notation: weak entity boxes are drawn with a **double-lined rectangle**; the identifying relationship diamond is **double-lined**; the partial key is **underlined with a dashed line**.

**Classic example**: `DEPENDENT` (a spouse or child of an employee, for insurance purposes) is weak. Its partial key might be `Dependent_Name`. Two different employees could each have a dependent named "Alice" — not unique globally — but combined with the owner `EMPLOYEE`'s key (say `SSN`), `(SSN, Dependent_Name)` becomes a full, globally unique key. So the *actual* primary key of a weak entity is always the **owner's key + the partial key**.

---

# 3. ER-to-Relational Mapping & ER Diagram Design

## 3.1 Refining the ER Design / Design Issues

When building a real ER diagram (the syllabus references a `COMPANY` database example — `EMPLOYEE`, `DEPARTMENT`, `PROJECT`, `DEPENDENT`), a few design choices recur:

- **Entity type vs. attribute**: Should "Department" be an entity type, or just an attribute of `EMPLOYEE`? Rule of thumb: if it has its own attributes worth tracking, or participates in relationships of its own, or many entities need to reference it (avoiding repetition), make it an entity type.
- **Entity type vs. relationship type**: Should "Works_On" be an entity type or a relationship? If it exists only to associate other entities and its own attributes (e.g., `Hours`) only make sense in the context of that association, it's a relationship type. But if it needs to participate in further relationships as a "thing" in its own right, it might warrant conversion to an entity type (this is the basis for converting M:N relationships-with-attributes into associative/"bridge" entities in relational mapping, see below).
- **Binary vs. n-ary relationships**: Prefer binary relationships where they capture full meaning; use ternary+ only when a real 3-way (or more) constraint exists that can't be decomposed into binary relationships without losing information.
- **Naming conventions**: Entity types get singular nouns (`EMPLOYEE`, not `EMPLOYEES`); relationship names are often verbs (`WORKS_FOR`, `MANAGES`); attribute names are nouns. Consistency here matters a lot for readability of large diagrams and is graded in coursework.

## 3.2 ER-to-Relational Mapping Algorithm

This is a *procedure* you're expected to be able to execute step by step. Given an ER diagram, produce a relational schema (a set of tables):

**Step 1 — Regular (strong) entity types.**
For each regular entity type `E`, create a relation `R` that includes all of `E`'s simple attributes (composite attributes get flattened into their simple components; only one attribute from each multivalued attribute is *not* included here — see Step 4). Choose one of `E`'s key attributes as the primary key of `R`.
> `EMPLOYEE(SSN, Name, Birth_date, Address, ...)` — primary key `SSN`.

**Step 2 — Weak entity types.**
For each weak entity type `W` with owner entity type `E` (already mapped to relation `R_E`), create relation `R_W` including all simple attributes of `W`, plus the primary key attributes of `R_E` as **foreign key** attributes in `R_W`. The primary key of `R_W` is the combination of the owner's primary key + `W`'s partial key.
> `DEPENDENT(Employee_SSN, Dependent_Name, Birth_date, Relationship)` — primary key `(Employee_SSN, Dependent_Name)`, foreign key `Employee_SSN → EMPLOYEE.SSN`.

**Step 3 — Binary 1:1 relationship types.**
Choose one of the two participating relations — preferably the one with **total participation** — and put the primary key of the *other* relation into it as a foreign key, along with any relationship attributes.
> If every `DEPARTMENT` has total participation in `MANAGES` (always has a manager), put `Manager_SSN` (FK to `EMPLOYEE`) inside `DEPARTMENT`.

**Step 4 — Binary 1:N relationship types.**
Put the primary key of the entity on the "1" side as a foreign key into the relation on the "N" side (plus relationship attributes there too). This is the most common case.
> `EMPLOYEE` works for one `DEPARTMENT`, a `DEPARTMENT` has many `EMPLOYEE`s → put `Dno` (FK to `DEPARTMENT`) inside `EMPLOYEE`.

**Step 5 — Binary M:N relationship types.**
Cannot be represented by a foreign key in either existing relation (would require multiple values). Instead, create a **new relation** `R` to represent the relationship, whose attributes are the primary keys of both participating relations (as foreign keys) plus any attributes of the relationship itself. The primary key of `R` is the combination of both foreign keys.
> `WORKS_ON(Essn, Pno, Hours)` — primary key `(Essn, Pno)`, both are foreign keys.

**Step 6 — Multivalued attributes.**
For each multivalued attribute `A` of entity type `E` (mapped to relation `R`), create a new relation `R_A` containing an attribute corresponding to `A`, plus the primary key attribute(s) of `R` as a foreign key. The primary key of `R_A` is the combination of both.
> `EMPLOYEE` has `{Dependent_names}`? → Actually usually multivalued like `{Locations}` for `DEPARTMENT`: `DEPT_LOCATIONS(Dnumber, Dlocation)`, primary key `(Dnumber, Dlocation)`.

**Step 7 — N-ary (degree > 2) relationship types.**
Same idea as M:N binary: create a new relation `R` with the primary keys of all participating entity types as foreign keys, plus relationship attributes. The primary key of `R` is usually the combination of all the foreign keys (unless cardinality constraints dictate a subset).

**Summary table:**

| ER construct | Relational mapping |
|---|---|
| Strong entity | Own table, PK = entity's key |
| Weak entity | Own table, PK = owner PK + partial key, FK to owner |
| 1:1 relationship | FK on either side (prefer the side with total participation) |
| 1:N relationship | FK placed on the "N" side |
| M:N relationship | New bridge/junction table with both PKs as composite PK |
| Multivalued attribute | New table: (owner PK, attribute value) |
| N-ary relationship | New table with all participants' PKs as FKs |

## 3.3 ER Diagram Notation Cheat-Sheet

| Symbol | Meaning |
|---|---|
| Rectangle | Entity type |
| Double rectangle | Weak entity type |
| Diamond | Relationship type |
| Double diamond | Identifying relationship |
| Oval | Attribute |
| Dashed oval | Derived attribute |
| Underlined attribute | Primary key |
| Underlined-dashed attribute | Partial key (discriminator) |
| Double oval / `{}` | Multivalued attribute |
| Double line | Total participation |
| Single line | Partial participation |

---

# 4. Relational Model & Relational Algebra

## 4.1 Relational Model Concepts

- **Relation**: a table — formally, a subset of the Cartesian product of a list of domains, but informally, "table with rows and columns."
- **Attribute**: a column, with a name and a domain.
- **Tuple**: a row — an ordered set of values, one per attribute.
- **Degree** of a relation: number of attributes (columns).
- **Cardinality**: number of tuples (rows) — this changes constantly as the DB is updated, unlike degree.
- **Relation schema** `R(A1, A2, ..., An)`: the name and attribute list — this is the *intension* (stable, part of the schema).
- **Relation state / instance** `r(R)`: the actual set of tuples at a point in time — the *extension* (changes with every INSERT/DELETE/UPDATE).
- A relation is technically a *set* of tuples → no duplicate tuples are (in theory) allowed, and ordering of tuples is not significant (SQL relaxes both of these in practice for efficiency and usability reasons — SQL tables can have duplicates unless you `SELECT DISTINCT`, and rows have no guaranteed order without `ORDER BY`).

## 4.2 Relational Model Constraints

- **Domain constraint**: every value of an attribute must be from its declared domain (atomic, single value from that type).
- **Key constraint**: no two tuples can have the same combination of values for the key attribute(s). Candidate keys, primary key, as discussed in Section 2.1 — the relational counterpart.
- **Entity integrity constraint**: no primary key attribute (or component of a composite primary key) may be `NULL`. This makes sense: the primary key is used to identify individual tuples, and a `NULL` there means "no identity."
- **Referential integrity constraint**: specified between two relations, used to maintain consistency among tuples of the two. Values of a **foreign key** in the referencing relation must either (a) match a value of the primary key in the referenced relation, or (b) be entirely `NULL`.

### Foreign key

A **foreign key (FK)** is a set of attributes in one relation `R1` that reference the primary key of another relation `R2` (possibly `R2 = R1`, i.e., self-referencing, like an employee's `Supervisor_SSN` referencing `EMPLOYEE.SSN`). Every FK value in `R1` must exist as a PK value in `R2` (or be NULL) — this is precisely what enforces referential integrity and is how relationships (1:N, weak entities, etc.) are actually implemented physically.

## 4.3 Update Operations and Constraint Violations

Three basic update operations — **Insert, Delete, Update** — and each can potentially violate one or more constraints:

| Operation | Can violate | What to do |
|---|---|---|
| **Insert** | Domain, key, entity integrity, referential integrity | Reject the insert, or in some systems, allow it to violate and flag it |
| **Delete** | Referential integrity (if the deleted tuple is referenced elsewhere) | Options: **restrict** (reject the delete), **cascade** (delete all referencing tuples too), **set null** (set the referencing FK to null) |
| **Update** | Domain, key (if updating PK), referential integrity (if updating FK, or updating a PK referenced elsewhere) | Similar options as delete — restrict/cascade/set null, depending on which attribute is updated |

This maps directly onto SQL's `ON DELETE CASCADE`, `ON DELETE SET NULL`, `ON DELETE RESTRICT` foreign-key clauses (see Section 5).

## 4.4 Relational Algebra

Relational algebra is a **procedural** query language — a set of operations that take one or two relations as input and produce a new relation as output. It's the formal foundation SQL is built on top of, and understanding it deeply makes understanding *how* SQL queries actually execute (Section 11) far more intuitive.

### Unary operations (operate on one relation)

**SELECT (σ)** — selects a *subset of rows (tuples)* satisfying a condition. Analogous to SQL's `WHERE`.
```
σ_(Salary > 50000) (EMPLOYEE)
```
Selects all employee tuples with salary greater than 50000.

**PROJECT (π)** — selects a *subset of columns (attributes)*, and (being a set operation) removes duplicate rows from the result. Analogous to SQL's `SELECT` column list (+ implicit `DISTINCT`).
```
π_(Name, Salary) (EMPLOYEE)
```

SELECT and PROJECT are often combined: `π_(Name) (σ_(Dno=5) (EMPLOYEE))` — names of employees in department 5.

**RENAME (ρ)** — renames a relation and/or its attributes, useful for self-joins and clarity. `ρ_S(B1,...,Bn)(R)`.

### Set-theoretic operations (require **union compatibility** — same number of attributes, matching domains)

| Operation | Symbol | Meaning |
|---|---|---|
| **Union** | ∪ | Tuples in R1 OR R2 (duplicates removed) |
| **Intersection** | ∩ | Tuples in BOTH R1 AND R2 |
| **Set difference** | − | Tuples in R1 but NOT in R2 (order matters! R1−R2 ≠ R2−R1) |
| **Cartesian product** | × | Every tuple of R1 combined with every tuple of R2 — produces R1's degree + R2's degree columns, and |R1|×|R2| rows |

### Binary relational operations — JOIN and DIVISION

**JOIN (⋈)** — combines related tuples from two relations into single tuples, based on a join condition. This is really Cartesian product + Selection, fused into one operation (and implemented far more efficiently — see join algorithms in Section 11).

- **Theta join** (⋈_θ): general join condition using any comparison operator (=, <, >, etc.)
- **Equijoin**: theta join where the condition uses only `=`. The joining attributes appear twice in the result (once from each relation).
- **Natural join** (⋈ or *): a special equijoin where (a) the join condition is equality on attributes with the *same name* in both relations, and (b) one copy of the join attribute(s) is automatically removed from the result. This is the most commonly used join in practice.
- **Outer joins**: preserve unmatched tuples (padding with `NULL`) instead of dropping them like a regular (inner) join does — **left outer join**, **right outer join**, **full outer join**. (Detailed comparison with examples in Section 12.)

**DIVISION (÷)** — a somewhat unusual but classically tested operator. `R ÷ S` finds tuples in R that are related to *every* tuple in S. Requires that S's attributes be a subset of R's.

Classic example: "Find employees who work on *all* projects that department 5 controls."
```
R = π_(Essn, Pno) (WORKS_ON)          -- (employee, project) pairs
S = π_(Pnumber) (σ_(Dnum=5) (PROJECT))  -- all project numbers controlled by dept 5
Result = R ÷ S                        -- employees who appear with EVERY project in S
```
Division has no direct SQL keyword — it's typically expressed in SQL using double negation ("no project in S is NOT worked on by this employee") — a good bridge into how "for all" queries are written in SQL (Section 5/12).

### Example queries in relational algebra (typical exam style)

Given `EMPLOYEE(Ssn, Name, Dno, Salary)`, `DEPARTMENT(Dnumber, Dname)`:

- *Retrieve names of employees who work in the 'Research' department:*
```
π_Name (σ_(Dname='Research') (EMPLOYEE ⋈_(Dno=Dnumber) DEPARTMENT))
```
- *Retrieve names of employees earning more than 60000:*
```
π_Name (σ_(Salary > 60000) (EMPLOYEE))
```

---

# 5. SQL

SQL (Structured Query Language) is the standard **declarative** language for relational databases — you say *what* you want, not *how* to get it (that's the optimizer's job — Section 11). It bundles together DDL, DML, and DCL.

## 5.1 SQL Data Definition (DDL)

```sql
CREATE TABLE DEPARTMENT (
    Dnumber   INT           PRIMARY KEY,
    Dname     VARCHAR(20)   NOT NULL UNIQUE,
    Mgr_ssn   CHAR(9),
    FOREIGN KEY (Mgr_ssn) REFERENCES EMPLOYEE(Ssn)
);

CREATE TABLE EMPLOYEE (
    Ssn       CHAR(9)       PRIMARY KEY,
    Name      VARCHAR(30)   NOT NULL,
    Salary    DECIMAL(10,2) CHECK (Salary > 0),
    Dno       INT,
    FOREIGN KEY (Dno) REFERENCES DEPARTMENT(Dnumber)
        ON DELETE SET NULL ON UPDATE CASCADE
);
```

- `CREATE TABLE`, `ALTER TABLE` (add/drop/modify columns), `DROP TABLE` — the schema-definition backbone.
- Basic data types: `CHAR(n)`, `VARCHAR(n)`, `INT`, `DECIMAL(p,s)`, `FLOAT`, `DATE`, `TIME`, `TIMESTAMP`, `BOOLEAN`.

## 5.2 Specifying Constraints in SQL

| Constraint | Purpose | Example |
|---|---|---|
| `NOT NULL` | Attribute can't be missing | `Name VARCHAR(30) NOT NULL` |
| `UNIQUE` | No two rows share this value (candidate key) | `Email VARCHAR(50) UNIQUE` |
| `PRIMARY KEY` | Unique + NOT NULL, identifies the row (entity integrity) | `Ssn CHAR(9) PRIMARY KEY` |
| `FOREIGN KEY ... REFERENCES` | Referential integrity | as above |
| `CHECK` | Arbitrary boolean condition on a row | `CHECK (Salary > 0)` |
| `DEFAULT` | Value used if none is supplied | `Status VARCHAR(10) DEFAULT 'active'` |
| `ON DELETE`/`ON UPDATE` `CASCADE`/`SET NULL`/`RESTRICT` | Referential action | as above |

## 5.3 Basic SQL Retrieval Queries

The canonical query skeleton:
```sql
SELECT [DISTINCT] column_list
FROM   table_list
WHERE  condition
ORDER BY column_list [ASC|DESC];
```

```sql
-- Simple selection + projection
SELECT Name, Salary FROM EMPLOYEE WHERE Dno = 5;

-- Join (implicit, via WHERE) -- old style
SELECT E.Name, D.Dname
FROM EMPLOYEE E, DEPARTMENT D
WHERE E.Dno = D.Dnumber;

-- Join (explicit, preferred style)
SELECT E.Name, D.Dname
FROM EMPLOYEE E JOIN DEPARTMENT D ON E.Dno = D.Dnumber;

-- Pattern matching, ranges, sets
SELECT Name FROM EMPLOYEE WHERE Name LIKE 'J%';
SELECT Name FROM EMPLOYEE WHERE Salary BETWEEN 40000 AND 80000;
SELECT Name FROM EMPLOYEE WHERE Dno IN (4, 5, 6);
SELECT Name FROM EMPLOYEE WHERE Salary IS NULL;   -- can't use "= NULL"!
```

**Note on NULL logic**: SQL uses **three-valued logic** — `TRUE`, `FALSE`, `UNKNOWN`. Any comparison involving `NULL` (e.g., `Salary = NULL`) yields `UNKNOWN`, not `TRUE`/`FALSE` — that's why you must use `IS NULL` / `IS NOT NULL`. A row is only returned by `WHERE` if the condition evaluates to `TRUE` (not `UNKNOWN`).

## 5.4 Insert, Delete, and Update Statements

```sql
INSERT INTO EMPLOYEE (Ssn, Name, Salary, Dno) VALUES ('123456789', 'Alice', 55000, 5);

DELETE FROM EMPLOYEE WHERE Salary < 20000;

UPDATE EMPLOYEE SET Salary = Salary * 1.10 WHERE Dno = 5;
```

## 5.5 More Complex SQL Retrieval Queries

```sql
-- Aggregate functions
SELECT COUNT(*), AVG(Salary), MAX(Salary), MIN(Salary), SUM(Salary)
FROM EMPLOYEE;

-- Grouping
SELECT Dno, AVG(Salary) AS avg_sal
FROM EMPLOYEE
GROUP BY Dno
HAVING AVG(Salary) > 50000;    -- HAVING filters GROUPS, WHERE filters ROWS (before grouping)

-- Nested query (subquery)
SELECT Name FROM EMPLOYEE
WHERE Dno IN (SELECT Dnumber FROM DEPARTMENT WHERE Dname = 'Research');

-- EXISTS
SELECT Name FROM EMPLOYEE E
WHERE EXISTS (SELECT * FROM DEPENDENT D WHERE D.Essn = E.Ssn);
```

`WHERE` vs `HAVING` is one of the most frequently tested distinctions: `WHERE` filters individual rows *before* aggregation; `HAVING` filters *groups* *after* aggregation (so `HAVING` can reference aggregate functions like `AVG()`, but plain `WHERE` cannot).

---

# 6. Relational Database Design — Functional Dependencies & Normalization

This unit answers the question: "Given a set of attributes, how do I organize them into 'good' tables?" It's arguably the single most heavily interview- and exam-tested topic in all of DBMS, so let's go slowly.

## 6.1 Why normalize? The problems with bad design

A poorly designed relation (one that crams too much unrelated information into a single table) suffers from **update anomalies**:

- **Insertion anomaly**: can't insert a fact without also having unrelated data available (e.g., can't record a new department's existence until it has at least one employee, if department and employee info are combined into one table).
- **Deletion anomaly**: deleting a fact accidentally destroys other, unrelated information (e.g., deleting the last employee in a department wipes out the department's name/location too).
- **Update anomaly**: a single logical fact is stored redundantly across multiple rows, so updating it means updating *every* copy — miss one, and the database becomes inconsistent (e.g., a department's location stored on every employee row in that department).

Normalization systematically removes these by decomposing relations, guided by **functional dependencies**.

## 6.2 Functional Dependencies (FDs)

**Definition**: Given a relation schema `R`, an FD `X → Y` (read "X functionally determines Y", or "Y is functionally dependent on X") means: for any two tuples `t1, t2` in any valid instance of `R`, if `t1[X] = t2[X]`, then `t1[Y] = t2[Y]`. In plain terms: **the value of X uniquely determines the value of Y** — X is like an "input" and Y a deterministic "output."

Example: `Ssn → Name` (an employee's SSN determines exactly one name). `{Ssn, Pnumber} → Hours` (combination of employee and project determines hours worked — Hours alone can't be determined by either attribute individually).

An FD is a *constraint*, derived from the real-world meaning/semantics of the attributes — not something you can discover just by looking at one instance of data; it must hold for *every possible* legal instance.

### Inference Rules (Armstrong's Axioms)

Given a set of FDs `F`, we can infer additional FDs that must also hold. These are the three fundamental, *sound and complete* rules:

1. **Reflexivity**: If `Y ⊆ X`, then `X → Y`. (Trivial FDs.)
2. **Augmentation**: If `X → Y`, then `XZ → YZ` for any Z.
3. **Transitivity**: If `X → Y` and `Y → Z`, then `X → Z`.

From these, several useful derived rules follow (you should be able to prove these using the three axioms above — a classic exam question):

4. **Union**: If `X → Y` and `X → Z`, then `X → YZ`.
5. **Decomposition**: If `X → YZ`, then `X → Y` and `X → Z`.
6. **Pseudotransitivity**: If `X → Y` and `WY → Z`, then `WX → Z`.

### Closure

- **Attribute closure X⁺**: the set of *all* attributes functionally determined by X, given F. Computed by repeatedly applying the inference rules until no new attributes can be added. Used to test whether `X → Y` is implied by F (check if `Y ⊆ X⁺`), and to find candidate keys (if `X⁺` = all attributes of R, X is a **superkey**; it's a candidate key if no subset of X also has this property).
- **Closure of F, F⁺**: the set of *all* FDs logically implied by F.

### Equivalence of Sets of FDs

Two sets of FDs `F` and `G` are **equivalent** if `F⁺ = G⁺` — i.e., every FD in F can be derived from G and vice versa. We say `G` **covers** `F` if every FD in F can be derived from G (a one-directional check, useful when building a minimal set).

### Minimal (Canonical) Sets of FDs

A set of FDs `Fmin` is **minimal** if:
1. Every FD's right-hand side is a *single* attribute.
2. No FD's left-hand side can be reduced (no **extraneous attributes** — removing any attribute from the LHS would produce a non-equivalent set).
3. No FD can be removed from the set entirely without losing equivalence (no **redundant FDs**).

Finding `Fmin` matters directly for normalization algorithms (e.g., 3NF synthesis) and dependency-preserving decomposition, discussed below.

## 6.3 Normal Forms Based on Primary Keys

Normal forms are progressively stricter rules a relation schema can satisfy. Higher normal form = fewer anomalies, generally at the cost of more tables (more joins needed at query time — a real design trade-off, not free).

### First Normal Form (1NF)

A relation is in 1NF if every attribute's domain contains only **atomic (indivisible) values**, and every attribute holds a **single value** (not a set/list) for each tuple. This *disallows*:
- Composite attributes (must be flattened into their components)
- Multivalued attributes (must be moved into a separate relation, as in ER-mapping Step 6)
- Nested relations (a table-within-a-table)

1NF is the baseline — every relation in the classic relational model is, by definition, required to be in 1NF.

### Second Normal Form (2NF)

Requires 1NF, plus: **every non-prime attribute is fully functionally dependent on the whole primary key** (not just part of it).

This only matters when the primary key is *composite* (more than one attribute). A violation is called a **partial dependency**: a non-prime attribute depends on only *part* of a composite key.

Example violation: `R(Ssn, Pnumber, Hours, Ename, Pname)` with PK `(Ssn, Pnumber)`.
- `Hours` depends on the *whole* key `(Ssn, Pnumber)` — fine.
- `Ename` depends only on `Ssn` (part of the key) — **partial dependency, violates 2NF**.
- `Pname` depends only on `Pnumber` (part of the key) — **also a partial dependency**.

Fix: decompose into `R1(Ssn, Pnumber, Hours)`, `R2(Ssn, Ename)`, `R3(Pnumber, Pname)`.

(Note: if a relation's primary key is a *single* attribute, it's automatically in 2NF, since there's no "part of the key" to depend on partially.)

### Third Normal Form (3NF)

Requires 2NF, plus: **no non-prime attribute is transitively dependent on the primary key.**

A **transitive dependency** exists when `X → Y` and `Y → Z` (Y is not a candidate key), so `X → Z` holds only "through" Y — Z depends on X *indirectly*.

Example violation: `EMPLOYEE(Ssn, Ename, Dno, Dname)` with PK `Ssn`.
- `Ssn → Dno` (direct)
- `Dno → Dname` (Dno determines the department name)
- So `Ssn → Dname` transitively, via `Dno` — **violates 3NF** (Dname doesn't depend on Ssn directly; it depends on the department).

Fix: decompose into `EMPLOYEE(Ssn, Ename, Dno)` and `DEPARTMENT(Dno, Dname)`.

### General (Codd's) Definitions of 2NF and 3NF

The definitions above (in terms of "the primary key") are the simplified textbook versions. The *general* definitions handle relations with **multiple candidate keys**:

- **Prime attribute**: an attribute that is part of *any* candidate key (not just the chosen primary key).
- **General 2NF**: no non-prime attribute is partially dependent on *any* candidate key (not just the primary key).
- **General 3NF**: for every non-trivial FD `X → A` where A is a non-prime attribute, either (a) X is a superkey, OR (b) A is a prime attribute. (This second clause — "or A is prime" — is exactly what BCNF removes, making BCNF strictly stronger.)

### Boyce-Codd Normal Form (BCNF)

A relation is in BCNF if for **every** non-trivial FD `X → Y` in the relation, `X` is a **superkey** — full stop, no "or Y is prime" exception.

BCNF is strictly stronger than 3NF. The gap between them: 3NF tolerates a dependency `X → A` where X is *not* a superkey, *as long as* A is prime (part of some candidate key) — BCNF does not allow this exception at all.

**Classic example where 3NF holds but BCNF doesn't**: `R(Student, Course, Instructor)` where:
- Each student can take a course from multiple instructors: `{Student, Course} → Instructor`
- But each instructor teaches only one course: `Instructor → Course`

Here `{Student, Course}` is a candidate key. But `Instructor → Course` has a non-superkey (`Instructor`) determining a prime attribute (`Course` is part of the candidate key) — this satisfies 3NF's exception clause, but violates BCNF, because `Instructor` is not a superkey.

**Trade-off to know**: decomposing into BCNF is always possible to eliminate redundancy, but it's not always possible to do so while **preserving all the original functional dependencies** (see 6.4) — sometimes you have to accept either a 3NF design (dependency-preserving but with minor residual redundancy) or a BCNF design (fully redundancy-free but where some FD can no longer be checked without a join). This is a genuinely famous trade-off — worth remembering by name.

### Comparison table

| Normal Form | Rule | Eliminates |
|---|---|---|
| 1NF | Atomic values only | Repeating groups / nested tables |
| 2NF | No partial dependency on the (whole) key | Redundancy from composite-key partial dependence |
| 3NF | No transitive dependency | Redundancy from indirect ("via another attribute") dependence |
| BCNF | Every determinant is a superkey | The last residual anomalies 3NF's "prime attribute" exception allows |

(4NF and 5NF exist too — dealing with multivalued dependencies and join dependencies respectively — often mentioned but rarely covered at the depth of 1NF–BCNF in an intro course; know they exist if asked "what comes after BCNF.")

## 6.4 Properties of Relational Decompositions

When you decompose a relation R into R1, R2, ..., Rn (to fix normal-form violations), the decomposition should ideally have two properties:

1. **Lossless (non-additive) join property**: joining R1, ..., Rn back together (via natural join) must reproduce *exactly* the original relation R — no spurious tuples, no lost information. This is **non-negotiable** — a lossy decomposition is simply a bad, incorrect decomposition, since you can never reconstruct the truth.
   - *Test (binary decomposition)*: R decomposed into R1, R2 is lossless iff `(R1 ∩ R2) → R1` or `(R1 ∩ R2) → R2` (the common attributes must be a superkey of at least one of the two pieces).
2. **Dependency-preservation property**: every FD in the original set F should be enforceable by checking constraints within *individual* decomposed relations, without needing to join relations back together to verify it. Desirable (so integrity checking stays cheap) but, unlike losslessness, **not always achievable simultaneously with BCNF** (see the BCNF trade-off above — 3NF synthesis always achieves both losslessness *and* dependency preservation; BCNF decomposition guarantees losslessness but may sacrifice dependency preservation).

---

# 7. Transaction Processing Concepts

## 7.1 Introduction to Transaction Processing

A **transaction** is a logical unit of database processing — a sequence of one or more read/write operations, executed as a single, atomic "all-or-nothing" unit. Classic example: a bank transfer — debit account A, credit account B — either *both* happen, or *neither* does; the database must never be left having done only one.

- **Read_item(X)**: reads a data item X from the database into a program variable.
- **Write_item(X)**: writes a program variable's value into data item X in the database.

**Concurrent execution** (interleaving operations from multiple transactions on a single-CPU or multi-CPU system) is done for performance/throughput, but introduces the entire problem of **concurrency control** — making sure interleaving doesn't corrupt data.

## 7.2 Transaction States and Additional Operations

Every transaction moves through a state diagram:

```
   BEGIN                                  COMMIT
     │                                      ▲
     ▼                                      │
  ┌──────┐   all operations executed   ┌──────────┐   commit success   ┌───────────┐
  │ACTIVE│ ───────────────────────────►│PARTIALLY │───────────────────►│ COMMITTED │
  └──┬───┘                             │COMMITTED │                    └───────────┘
     │                                 └────┬─────┘
     │ failure/error                        │ failure during commit
     ▼                                       ▼
  ┌───────┐                            ┌───────────┐
  │ FAILED│◄───────────────────────────┤(rolls back)│
  └───┬───┘                            └───────────┘
      │
      ▼
 ┌───────────┐
 │ TERMINATED│
 └───────────┘
```

- **Active**: initial state, executing.
- **Partially committed**: after the *final* operation executes, but before the commit is fully confirmed (e.g., data might still be in a buffer, not yet permanently on disk).
- **Committed**: transaction completed successfully, effects permanently recorded.
- **Failed**: normal execution can no longer proceed (e.g., a constraint violation, a system crash).
- **Terminated**: transaction has exited the system (after commit, or after a rollback from failed state).
- **ROLLBACK / ABORT**: undoes any changes the failed transaction had made, restoring the database to how it was before the transaction started.

Additional operations tracked for recovery purposes: `COMMIT`, `ROLLBACK`, and internally, the DBMS also tracks `UNDO` and `REDO` (see Section 14).

## 7.3 Desirable Properties of Transactions — ACID

The property set every transaction should ideally guarantee. **Know each letter cold with an example — this is asked in essentially every DBMS interview.**

| Property | Meaning | What breaks without it |
|---|---|---|
| **Atomicity** | All operations execute, or none do — "all or nothing" | Bank transfer debits A but crashes before crediting B → money vanishes |
| **Consistency** | A transaction takes the database from one *valid* state to another, preserving all declared constraints | A transaction could leave `balance < 0` when a CHECK constraint forbids it |
| **Isolation** | Concurrent transactions appear to execute as if they ran one at a time (serially), even though they're actually interleaved | Transaction A reads a value transaction B hasn't finished updating → "dirty read" |
| **Durability (permanency)** | Once committed, changes survive any subsequent failure (crash, power loss) | A commit acknowledged to the user, then a crash loses the write — unacceptable |

Note that Atomicity and Durability are primarily the responsibility of the **recovery manager** (Section 14); Isolation is primarily the responsibility of the **concurrency control** subsystem (Sections 8 & 15); Consistency is a shared responsibility between the application (correct transaction logic) and the DBMS (constraint enforcement).

## 7.4 Schedules of Transactions

A **schedule (history)** `S` is an ordering of the operations of one or more transactions, respecting the internal order of operations within each individual transaction.

- **Serial schedule**: transactions execute one completely after another, no interleaving at all. Guaranteed correct (since each transaction, assumed correct on its own, runs in total isolation) — but offers **zero concurrency**, hence poor performance.
- **Non-serial (interleaved) schedule**: operations from different transactions are interleaved. Better performance, but *might* cause incorrect results if not controlled carefully.

The entire goal of concurrency control is: **allow non-serial schedules for performance, but only ones that are guaranteed to produce the same result as *some* serial schedule.** That guarantee is called **serializability**.

## 7.5 Serializability

- A schedule `S` is **serializable** if it is **equivalent** to some serial schedule of the same transactions. "Equivalent" needs a precise definition — the standard one used in coursework is **conflict equivalence**.
- Two operations **conflict** if: (a) they belong to different transactions, (b) they access the *same* data item, and (c) at least one of them is a `write`. (So: read-read never conflicts; read-write and write-write on the same item do.)
- Two schedules are **conflict equivalent** if they have the same operations and order every pair of conflicting operations the same way.
- A schedule is **conflict serializable** if it is conflict equivalent to some serial schedule.

### Testing for Conflict Serializability — the Precedence Graph method

Build a directed graph:
1. One node per transaction.
2. For every pair of conflicting operations `Ti: op1(X)` and `Tj: op2(X)` where `op1` occurs before `op2` in the schedule, draw an edge `Ti → Tj` ("Ti must precede Tj in any equivalent serial order").

**A schedule is conflict serializable if and only if its precedence graph has no cycle.** If acyclic, a topological sort of the graph gives you a valid equivalent serial order (there may be more than one if the graph allows multiple valid topological sorts).

This graph-cycle test is a guaranteed exam/interview question — practice constructing one by hand from a given interleaved schedule.

---

# 8. Concurrency Control Techniques (Basic)

The purpose of concurrency control is to ensure isolation and serializability of concurrently executing transactions, while allowing as much concurrency (parallelism) as possible for performance.

## 8.1 Locking

A **lock** is a variable associated with a data item, controlling access to it by concurrent transactions.

- **Binary locks**: simple locked/unlocked — too restrictive in practice (doesn't distinguish read vs write access), rarely used directly.
- **Shared/Exclusive (read/write) locks** — the standard scheme:
  - **Shared (S) lock**: multiple transactions may hold a shared lock on the same item simultaneously — used for reading. Read-read doesn't conflict, so this is safe.
  - **Exclusive (X) lock**: only one transaction may hold it, and no other transaction may hold *any* lock (S or X) on that item at the same time — used for writing.
  - **Lock compatibility matrix**:

| | S requested | X requested |
|---|---|---|
| **S held** | ✅ Granted | ❌ Wait |
| **X held** | ❌ Wait | ❌ Wait |

## 8.2 Two-Phase Locking (2PL)

**2PL Rule**: every transaction must be divided into exactly two phases:
1. **Growing phase**: the transaction may *acquire* locks, but may not release any.
2. **Shrinking phase**: the transaction may *release* locks, but may not acquire any new ones.

**Theorem**: If every transaction in a schedule follows the 2PL protocol, the schedule is guaranteed to be conflict serializable. (This is the single most important theorem of the whole topic — 2PL is *sufficient* but not *necessary* for serializability; there exist serializable schedules that don't follow 2PL.)

**Basic 2PL's problem**: it can still lead to **cascading rollback** — since locks can be released before commit (during the shrinking phase, while the transaction is still running), another transaction might read a value written by an as-yet-uncommitted transaction; if that first transaction then aborts, everyone who read its "dirty" value must also be rolled back, and so on, in a cascade. (Solutions: strict/rigorous 2PL, covered in Section 15.)

**Deadlock risk**: 2PL guarantees serializability but does *not* prevent **deadlock** — two transactions each waiting on a lock the other holds, neither able to proceed. (Deadlock detection/prevention detailed in Section 13.)

## 8.3 Types of Locks and Lock Tables

- **Lock granularity**: locks can be applied at different levels — the whole database, a table, a page/block, a specific tuple (row), or even a single field. Finer granularity (row-level) → more concurrency but more locking overhead (more locks to manage); coarser granularity (table-level) → less overhead but less concurrency. Systems often use **multiple granularity locking** with **intention locks** (IS, IX) to efficiently signal "I intend to lock something finer-grained below this node" without checking every descendant.
- **System lock table**: an in-memory hash table the DBMS uses to track which transaction holds which lock on which item, and the current queue of waiting requests. Lookups happen on essentially every read/write, so this table's implementation is performance-critical.

---

# 9. NoSQL & Big Data

## 9.1 Why NoSQL? Motivation

Relational databases assume a single, fixed schema and (traditionally) scale **vertically** (bigger machine). As web-scale applications emerged (huge, rapidly changing, semi-structured data, needing to scale **horizontally** across many cheap commodity machines), a family of alternative data stores — collectively "**NoSQL**" ("Not Only SQL") — emerged, generally trading some of the relational model's strict consistency/structure guarantees for scalability, flexibility, and availability.

## 9.2 Aggregate Data Models

An **aggregate** is a collection of related data that you tend to treat as a single unit for data manipulation — e.g., "a customer and their orders and their shipping addresses" grouped together, instead of split across normalized tables joined at query time. NoSQL databases are largely organized around this concept — data is often **denormalized** by design.

- **Key-value model**: the simplest — a big distributed hash map; each aggregate is stored as an opaque "value" retrievable only by its unique "key." The database itself doesn't understand the value's internal structure. *Example systems*: Redis, Riak, DynamoDB (originally).
- **Document model**: like key-value, but the "value" (the **document**, typically JSON/BSON/XML) has visible, query-able structure — the database can index and query *into* fields within the document, not just fetch by key. *Example systems*: MongoDB, CouchDB.
- **Column-family (wide-column) model**: data organized into rows, but *within* a row, columns are grouped into "column families," and different rows can have different columns present — a middle ground between relational tables and pure key-value. *Example systems*: Cassandra, HBase, Bigtable. (Covered further in Section 19.)
- **Graph model**: (a fourth, non-aggregate-oriented category, usually mentioned alongside these) models data as nodes and edges — optimized for relationship-heavy queries (e.g., social networks, recommendation engines). *Example systems*: Neo4j.

## 9.3 Distribution Models

**Sharding (horizontal partitioning)**: splitting a dataset across multiple servers, where *each* server holds a different *subset* of the total data (as opposed to a full copy). Improves both read and write scalability, since load is spread across machines — but a single server going down loses access to its shard (unless combined with replication).

**Replication**: keeping *multiple copies* of the same data on different servers, for availability and read scalability.
- **Master-slave replication**: one node (master) accepts all writes and propagates them to replica ("slave"/"follower") nodes; slaves usually only serve reads. Simple, and consistent for reads *of the master*, but the master is a single point of failure for writes, and reads from slaves may be **stale** (not yet caught up with the master).
- **Peer-to-peer (masterless) replication**: all nodes accept both reads and writes, and propagate changes to each other. No single point of failure, better write availability/scalability — but higher risk of write-write **conflicts** between nodes needing resolution (e.g., "last write wins," or vector clocks, or application-level merge logic).

**Combining sharding and replication**: the two techniques aren't mutually exclusive — a real production cluster commonly shards the dataset across many groups of servers, *and* replicates each shard (often with a master-slave setup per shard) so that each individual shard also tolerates node failure. This "sharded + replicated" pattern is how most large-scale NoSQL deployments (Cassandra, MongoDB replica sets + sharding) actually run.

## 9.4 Big Data — Types of Data

| Type | Description | Example |
|---|---|---|
| **Structured** | Fixed schema, fits neatly into rows/columns | A relational sales table |
| **Semi-structured** | Has some organizational structure/tags, but no rigid fixed schema | JSON, XML, email (has headers + free-text body) |
| **Unstructured** | No predefined structure at all | Free text, images, video, audio, social-media posts |

## 9.5 Distributed Architecture — Hadoop & MapReduce

**Hadoop** is an open-source framework for distributed storage and processing of very large datasets across clusters of commodity hardware, built on two core pillars:

- **HDFS (Hadoop Distributed File System)**: splits large files into fixed-size **blocks** (traditionally 64MB or 128MB), distributed and **replicated** (typically 3x) across many machines in the cluster, for both fault tolerance and data locality (moving *computation* to where the *data* already lives, rather than moving huge datasets over the network).
  - **NameNode**: master node, manages the filesystem namespace/metadata (which blocks belong to which file, and where each block's replicas live).
  - **DataNodes**: workers that actually store the blocks.

- **MapReduce Programming Model**: a two-phase model for processing huge datasets in parallel across a cluster:
  1. **Map phase**: a user-defined `map(key, value) → list(intermediate_key, intermediate_value)` function is applied, in parallel, to chunks of the input data independently across the cluster.
  2. **Shuffle & Sort**: the framework automatically groups all intermediate values by their intermediate key, and distributes each key's group to a reducer.
  3. **Reduce phase**: a user-defined `reduce(intermediate_key, list(intermediate_values)) → list(output_values)` function aggregates each key's group into final output.

  **Classic example — word count**: `map` emits `(word, 1)` for every word in a document; `reduce` sums up the 1's for each word to get the total count. This pattern — independent parallel "map" work followed by an aggregating "reduce" — generalizes to a huge range of big-data batch-processing tasks (though it's since been complemented/succeeded in practice by faster in-memory frameworks like Apache Spark, worth knowing exists even though it's outside your syllabus proper).

---

---

# 10. Indexing & File Organization

*"How does the database search fast?" — this is probably the single most common systems-level DBMS interview question, and it's not in your syllabus at all, so pay close attention here.*

## 10.1 File Organizations (how records are physically stored)

- **Heap (unordered) file**: records placed wherever there's free space, in no particular order. Insertion is O(1) — just append. But search (unless there's an index) requires a full linear scan, O(n).
- **Sequential (ordered) file**: records physically sorted on disk by some field (the **ordering field**). Search on that field can use **binary search**, O(log n) — much faster. But insertion is expensive: maintaining sort order means shifting records (or using overflow areas that eventually need reorganizing).
- **Hash file**: a hash function maps a record's key to a **bucket** (a physical disk block/page) where it's stored. Lookup by key is O(1) average case — extremely fast for exact-match queries, but useless for range queries (`WHERE age > 30`) since hashing destroys ordering.

## 10.2 Why Indexes? The core idea

An **index** is an *auxiliary* access structure — separate from the data file itself — that speeds up retrieval of records satisfying a search condition, at the cost of extra storage and the overhead of maintaining the index whenever the underlying data changes (every INSERT/UPDATE/DELETE must also update the index).

## 10.3 Types of Indexes

**Primary vs. Secondary Index**
- **Primary index**: built on the ordering (primary) key field of a file that is itself physically sorted by that field. There's at most one primary index per file (since a file can only be physically sorted one way).
- **Secondary index**: built on any *non-ordering* field (could be a candidate key, or a non-key field). A file can have many secondary indexes. Because the file isn't sorted by this field, a secondary index has to have an entry for *every* record (not one per block), making it larger than an equivalent primary index.

**Clustered vs. Non-clustered Index** — an important, related-but-distinct distinction (and a common source of confusion; make sure to keep it separate from primary/secondary above):
- **Clustered (clustering) index**: the actual data rows are physically stored in the *same order* as the index. A table can have **only one** clustered index (there's only one physical order), and it's often built automatically on the primary key.
- **Non-clustered index**: a separate structure that stores pointers to the actual data rows, which remain in whatever (possibly unrelated) physical order. A table can have **many** non-clustered indexes. Lookups require one extra hop (index → pointer → actual row), unlike a clustered index where the leaf level *is* the data.

**Dense vs. Sparse Index**
- **Dense index**: has an index entry for **every** search-key value (and hence every record) in the data file.
- **Sparse (non-dense) index**: has an index entry for only **some** of the search-key values, typically one per data block/page (used for primary indexes on sorted files — since records within a block are already sorted, you only need to know the first key of each block to binary-search your way in, then scan the block).

## 10.4 Data Structures Used for Indexing

### B-Trees / B+ Trees — the single most important data structure in this whole topic

A **B-Tree** is a self-balancing, multi-way search tree, designed so tree height stays very small even for millions of records (because each node can hold many keys/children — matched to a disk block size, so one node read = one disk I/O). This directly minimizes the number of expensive disk accesses needed per search.

**B+ Tree** (the variant almost every real DBMS actually uses):
- All actual data (or pointers to data rows) is stored **only at the leaf level**. Internal nodes hold **only keys**, used purely for navigation/routing — no data.
- Leaf nodes are additionally **linked together** in a sorted linked list — this makes **range queries** (`BETWEEN`, `>`, `ORDER BY`) extremely efficient: find the start of the range with one tree descent, then just walk the linked leaves.
- Search, insertion, and deletion are all **O(log_b n)**, where `b` is the branching factor (often in the hundreds, given typical disk-block sizes) — so even a tree over a billion rows might only be 3-4 levels deep.

Why B+ Tree over plain Binary Search Tree for a database index? A BST is O(log₂ n) too — but base-2 means far *more levels* for the same number of keys, and each level-traversal in a disk-backed structure is a (slow) disk seek. B+ Trees maximize the **branching factor** to minimize tree height and thus minimize disk I/Os — this "optimize for disk I/O, not just comparison count" mindset is the key insight interviewers are checking for.

### Hash-based Indexing

Uses a hash function to compute a bucket address directly from the key — O(1) average lookup for exact-match (`=`) queries. Two flavors worth knowing by name:
- **Static hashing**: fixed number of buckets, chosen up front — degrades badly if the data grows far beyond initial estimates (overflow chaining, or a costly full rehash).
- **Dynamic/Extendible hashing**: the hash structure grows/shrinks incrementally as data grows, avoiding the "one giant rehash" problem.

**B+ Tree vs. Hash index — the question you WILL be asked**: hashing wins for exact-match/point lookups (`WHERE id = 5`); B+ Trees win whenever ordering matters — range queries, `ORDER BY`, `BETWEEN`, `<`/`>` — because hashing scatters keys with no preserved order, while a B+ Tree's sorted leaf chain directly supports these. Most general-purpose relational databases default to B+ Tree indexes for exactly this reason (range queries are common); hash indexes show up more in specialized/pure key-value contexts.

---

# 11. Query Processing & Optimization

*You learned to **write** SQL. This section covers what the database does with it after you hit "execute" — a favorite "let's see if you actually understand databases" interview angle.*

## 11.1 The Query Processing Pipeline

```
SQL query text
     │
     ▼
1. Parsing & Translation  — check syntax, verify tables/columns exist,
     │                       translate into an internal relational-algebra
     │                       expression / query tree
     ▼
2. Query Optimization      — the optimizer considers multiple equivalent
     │                       execution plans and estimates the cost of each
     │                       (using statistics: table sizes, index availability,
     │                       data distribution/histograms)
     ▼
3. Query Code Generation   — the chosen (lowest-cost) plan is compiled into
     │                       an executable/interpretable form
     ▼
4. Execution (Runtime)     — the plan actually runs against the stored data,
                              producing the result
```

## 11.2 Heuristic (Rule-Based) Optimization

Applies general rules that almost always reduce cost, regardless of actual data statistics — cheap to apply, doesn't require accurate stats:

- **Selection (and projection) pushdown**: move `σ` (WHERE-filters) and `π` (column projections) as *early* as possible in the query tree — ideally right after the base table scan — so later, more expensive operations (especially joins) work on the smallest possible intermediate relation.
- **Join reordering**: among several joins, execute the ones that produce the *smallest* intermediate results first, to keep every subsequent operation cheap.
- Combine (cascade) multiple `σ`/`π` operations into a single pass over the data where possible.

## 11.3 Cost-Based Optimization

Goes further: the optimizer estimates the actual **cost** (usually dominated by disk I/O count, sometimes also CPU) of multiple *candidate* execution plans for the same query, using statistics maintained in the catalog (table cardinalities, index selectivity, value distributions/histograms), and picks the plan with lowest estimated cost. This is why the *same* SQL query can run with a completely different physical execution strategy depending on current table sizes and available indexes — the query is declarative; the plan is not.

## 11.4 Join Algorithms — how a JOIN is actually executed

| Algorithm | How it works | Best when |
|---|---|---|
| **Nested Loop Join** | For each tuple in the outer (typically smaller) relation, scan the *entire* inner relation looking for matches. Simplest, always works, but O(n×m) in the worst case | No usable index, and at least one relation is small |
| **Block Nested Loop Join** | Optimization of the above: process the outer relation a whole disk-block at a time rather than tuple-by-tuple, cutting down disk I/Os significantly | Similar to plain nested loop, but with less I/O overhead |
| **Sort-Merge Join** | Sort both relations on the join attribute (if not already sorted/indexed that way), then merge them in a single linear pass, like merging two sorted lists | Both relations are large but can be sorted (or are already sorted/indexed), and the join is an equijoin |
| **Hash Join** | Build an in-memory hash table on the join attribute of the smaller relation ("build" phase), then scan the larger relation ("probe" phase), looking up matches in the hash table | Equijoins, no useful sort order/index already exists, and the smaller relation fits (or mostly fits) in memory |

Interview one-liner: *Nested loop* is the fallback with no index; *sort-merge* shines when data is already sorted or you need a sorted output anyway; *hash join* is usually fastest for large unsorted equijoins when memory allows building the hash table.

---

# 12. Advanced SQL

## 12.1 Joins — Deep Dive

```sql
-- INNER JOIN: only matching rows from both sides
SELECT e.Name, d.Dname FROM EMPLOYEE e INNER JOIN DEPARTMENT d ON e.Dno = d.Dnumber;

-- LEFT (OUTER) JOIN: all rows from the left table, matched rows from the right
-- (unmatched right-side columns come back as NULL) -- e.g., "all employees, even ones with no department"
SELECT e.Name, d.Dname FROM EMPLOYEE e LEFT JOIN DEPARTMENT d ON e.Dno = d.Dnumber;

-- RIGHT (OUTER) JOIN: mirror image -- all rows from the right table
SELECT e.Name, d.Dname FROM EMPLOYEE e RIGHT JOIN DEPARTMENT d ON e.Dno = d.Dnumber;

-- FULL (OUTER) JOIN: all rows from both sides, NULLs padded wherever there's no match
SELECT e.Name, d.Dname FROM EMPLOYEE e FULL JOIN DEPARTMENT d ON e.Dno = d.Dnumber;
```

## 12.2 Subqueries — Correlated vs. Non-correlated

- **Non-correlated subquery**: can be evaluated **once**, independently of the outer query, and its result reused.
```sql
SELECT Name FROM EMPLOYEE
WHERE Salary > (SELECT AVG(Salary) FROM EMPLOYEE);
```
- **Correlated subquery**: references a column from the *outer* query, so it must be re-evaluated **once per row** of the outer query — generally more expensive.
```sql
SELECT e.Name FROM EMPLOYEE e
WHERE e.Salary > (SELECT AVG(Salary) FROM EMPLOYEE e2 WHERE e2.Dno = e.Dno);
-- "employees earning more than their OWN department's average"
```

## 12.3 Views

A **view** is a virtual table defined by a stored query — it has no independently stored data of its own (usually); it's recomputed (or, if the DB supports it, incrementally maintained) from the underlying base tables each time it's queried.
```sql
CREATE VIEW HighEarners AS
SELECT Name, Salary, Dno FROM EMPLOYEE WHERE Salary > 80000;
```
Used for: simplifying complex queries for end users, restricting access to a subset of columns/rows (security), and providing logical data independence (Section 1.5) — the view's definition can be adjusted if the base schema changes, without breaking the applications that query the view.

## 12.4 GROUP BY + HAVING (recap with nuance)

`GROUP BY` collapses rows sharing the same value(s) in specified columns into groups, so aggregate functions operate per group instead of over the whole table. `HAVING` filters *those groups* using a condition possibly involving an aggregate — something plain `WHERE` cannot do, since `WHERE` is applied before grouping even happens.

## 12.5 Window Functions — very important, very commonly tested now

Unlike `GROUP BY` (which collapses rows), window functions compute a value **for each row**, based on a "window" of related rows, **without** collapsing the result set.

```sql
SELECT Name, Dno, Salary,
       ROW_NUMBER() OVER (PARTITION BY Dno ORDER BY Salary DESC) AS rn,
       RANK()       OVER (PARTITION BY Dno ORDER BY Salary DESC) AS rnk,
       DENSE_RANK() OVER (PARTITION BY Dno ORDER BY Salary DESC) AS drnk
FROM EMPLOYEE;
```

- `PARTITION BY` — splits rows into groups (like `GROUP BY`, but doesn't collapse them).
- `ORDER BY` (inside `OVER`) — defines ordering *within* each partition.
- `ROW_NUMBER()` — assigns 1, 2, 3, 4... with **no ties** (even if two salaries are equal, they get different numbers, arbitrarily broken).
- `RANK()` — ties get the *same* rank, but the **next** rank *skips* (1, 1, 3, 4...).
- `DENSE_RANK()` — ties get the same rank, and the next rank does **not** skip (1, 1, 2, 3...).

This trio's exact tie-breaking difference is a guaranteed interview question — memorize the (1,1,3) vs (1,1,2) distinction.

## 12.6 CTEs (Common Table Expressions) — the `WITH` clause

A named, temporary result set, scoped to a single query — improves readability over deeply nested subqueries, and enables **recursive** queries (e.g., traversing an employee-supervisor hierarchy):
```sql
WITH DeptAvg AS (
    SELECT Dno, AVG(Salary) AS avg_sal FROM EMPLOYEE GROUP BY Dno
)
SELECT e.Name, e.Salary, d.avg_sal
FROM EMPLOYEE e JOIN DeptAvg d ON e.Dno = d.Dno
WHERE e.Salary > d.avg_sal;
```

## 12.7 Triggers & Stored Procedures (brief — full practical depth in Section 21)

- A **stored procedure** is precompiled, reusable, named SQL/procedural code stored and executed inside the DBMS itself.
- A **trigger** is a stored procedure that *automatically* fires in response to a specific database event (`INSERT`/`UPDATE`/`DELETE`) on a specific table — used for enforcing complex business rules, auditing, or maintaining derived/denormalized data automatically.

---

# 13. Transactions — Advanced Depth

## 13.1 ACID Properties — Deeper Look

(Refer back to Section 7.3 for the base definitions.) The key deeper insight for interviews: **Atomicity and Durability are the recovery manager's job** (undo/redo logging — Section 14); **Isolation is the concurrency control subsystem's job** (locking/MVCC — Sections 8 & 15); **Consistency is a shared contract** — the DBMS enforces declared constraints, but the application must write transaction logic that, given a consistent starting state and no interference, produces a consistent ending state.

## 13.2 Recoverability of Schedules

Separate from *serializability* (which is about correctness under concurrency) is **recoverability** (about whether commits/aborts can be handled safely at all):

- **Recoverable schedule**: if a transaction `Tj` reads a data item previously written by `Ti`, then `Ti`'s commit must occur **before** `Tj`'s commit. In other words: you never let a transaction commit based on data written by a transaction that might still abort. Non-recoverable schedules are simply unacceptable — if `Ti` aborts after `Tj` already committed having read `Ti`'s uncommitted write, there's no way to undo `Tj` (it's already permanent), leaving corrupted, unrecoverable data.
- **Cascading rollback (cascading abort)**: even in a *recoverable* schedule, if `Tj` reads `Ti`'s uncommitted write and `Ti` later aborts, `Tj` must *also* be aborted (since it read bad data) — and if some `Tk` read from `Tj`, it cascades further. This is wasteful and undesirable, though not *incorrect* per se.
- **Cascadeless schedule**: avoids this entirely, by requiring that every transaction can only read values that were written by *already-committed* transactions (no "dirty reads" allowed at all).
- **Strict schedule**: even stronger — a transaction can't read *or write* an item until the transaction that last wrote it has committed or aborted. Strict schedules are the easiest to implement `UNDO` for during recovery (you know for certain that any value on disk written by a still-active transaction is exclusively that transaction's — nobody else could have overwritten it since).

Nesting relationship: **Strict ⊂ Cascadeless ⊂ Recoverable** (every strict schedule is cascadeless; every cascadeless schedule is recoverable; the reverse doesn't hold).

## 13.3 Deadlocks

A **deadlock** occurs when two (or more) transactions are each waiting for a lock held by the other, so neither can ever proceed. Classic example: T1 holds a lock on X and wants Y; T2 holds a lock on Y and wants X — both wait forever.

**Deadlock Detection** — allow deadlocks to happen, then find and break them:
- **Wait-for graph**: a directed graph with one node per transaction; draw an edge `Ti → Tj` if `Ti` is waiting for a lock held by `Tj`. **A deadlock exists if and only if this graph has a cycle.** The DBMS periodically checks for cycles and, if found, picks a **victim** transaction to abort (breaking the cycle) — usually chosen to minimize total rollback cost (e.g., the youngest transaction, or the one with the fewest changes made so far).

**Deadlock Prevention** — disallow the conditions that could lead to deadlock in the first place, typically using transaction **timestamps** to decide whether a transaction should wait or be aborted when it requests a lock held by another:
- **Wait-Die** ("older waits, younger dies"): if the requesting transaction `Ti` is **older** than the lock-holder `Tj`, `Ti` is allowed to *wait*. If `Ti` is **younger**, `Ti` is aborted ("dies") and restarted later (with the *same* original timestamp, so it eventually becomes the oldest and can't be perpetually starved).
- **Wound-Wait** ("older wounds, younger waits"): if the requesting transaction `Ti` is **older** than the lock-holder `Tj`, `Tj` is forced to abort ("wounded") and restart, and `Ti` takes the lock. If `Ti` is **younger**, `Ti` simply *waits*.

Both schemes guarantee no deadlock (since one of the two transactions is always forced to give way rather than mutually block), and both avoid starvation by preserving original timestamps on restart — but they differ in which one (younger/older) is forced to yield, which affects performance characteristics under different workloads.

## 13.4 Isolation Levels — extremely important, extremely commonly asked

SQL defines four standard isolation levels, offering a trade-off between correctness (fewer anomalies) and concurrency (performance). Each level is defined by which of three "read phenomena" it permits:

- **Dirty read**: reading a value written by an uncommitted transaction (which might later abort).
- **Non-repeatable read**: re-reading the *same row* within one transaction gives a *different* value, because another transaction committed an update to it in between.
- **Phantom read**: re-running the *same range query* within one transaction returns a *different set of rows*, because another transaction inserted/deleted rows matching that range in between.

| Isolation Level | Dirty Read | Non-repeatable Read | Phantom Read |
|---|---|---|---|
| **Read Uncommitted** | ✅ Possible | ✅ Possible | ✅ Possible |
| **Read Committed** | ❌ Prevented | ✅ Possible | ✅ Possible |
| **Repeatable Read** | ❌ Prevented | ❌ Prevented | ✅ Possible |
| **Serializable** | ❌ Prevented | ❌ Prevented | ❌ Prevented |

- **Read Uncommitted**: virtually no isolation — can read other transactions' uncommitted (dirty) writes. Rarely used; occasionally for pure approximate reporting where speed matters more than accuracy.
- **Read Committed**: the most common **default** in many real systems (e.g., PostgreSQL, Oracle) — only ever reads committed data, but re-reading the same row later in the same transaction can see a newer committed value.
- **Repeatable Read**: (MySQL/InnoDB's default) guarantees that if you read a row once, re-reading it later in the same transaction gives the *same* value — but new rows matching a range condition can still appear (phantoms), *in the strict textbook definition* (note: MySQL's actual `REPEATABLE READ` implementation, using MVCC/gap-locks, additionally prevents phantoms in practice — a nuance worth mentioning if it comes up, but the textbook/ANSI table above is what's tested).
- **Serializable**: the strongest — behaves as if transactions ran one at a time, in some serial order. Prevents all three anomalies, at the cost of the least concurrency (most locking/blocking, or most abort-and-retry under optimistic schemes).

---

---

# 14. Database Recovery Techniques

*How does a database survive a crash without losing committed work or keeping uncommitted garbage? This underpins Atomicity + Durability from ACID.*

## 14.1 Log-Based Recovery — Write-Ahead Logging (WAL)

The DBMS maintains a **system log** — a sequential, append-only file recording every database update, before the update is applied to the actual data on disk. Each log record typically includes: transaction ID, data item, old value (**before-image**, used for UNDO), new value (**after-image**, used for REDO).

**The Write-Ahead Logging (WAL) rule**: the log record for an update **must** be written to stable (non-volatile) storage **before** the corresponding data update itself is written to disk. This is the foundational rule that makes recovery possible at all — if the system crashes mid-write, the log (guaranteed to be safely on disk already) tells the recovery process exactly what was in progress and how to fix it.

## 14.2 UNDO and REDO Operations

- **UNDO**: uses the log's before-images to *reverse* the effects of transactions that were **active (uncommitted) at the time of the crash** — since their changes must not persist (Atomicity).
- **REDO**: uses the log's after-images to *re-apply* the effects of transactions that **committed** but whose changes might not have made it from memory buffers to disk before the crash — since committed changes must persist (Durability).

On restart after a crash, the recovery process scans the log and classifies every transaction as either needing UNDO (was active, never committed) or REDO (committed, but maybe not fully flushed to disk) — then applies the appropriate operations.

## 14.3 Checkpoints

Scanning the *entire* log from the beginning of time on every crash would be prohibitively slow. A **checkpoint** is a periodic marker the DBMS writes to the log, at which point: (a) all buffers are force-written to disk, and (b) a record of all transactions active at that moment is saved. On recovery, the process only needs to scan the log **back to the most recent checkpoint** (rather than the entire history), since everything before that point is guaranteed already durable.

## 14.4 Shadow Paging

An alternative to log-based recovery (used less often in practice today, but a classic exam topic). Instead of logging changes, the database maintains two page tables:
- The **current page table**, which the active transaction modifies (writing changes to *new* disk pages, never overwriting old ones in place).
- The **shadow page table**, a copy of the current table *before* the transaction started, kept untouched.

If the transaction commits, the current page table atomically *becomes* the new shadow page table (a single pointer swap). If it aborts (or the system crashes mid-transaction), the shadow table — which was never touched — is simply still valid, and the partially updated pages are just discarded/garbage-collected. This gives atomicity without needing UNDO logging at all, since nothing is ever overwritten in place — but it suffers from **data fragmentation** (updated pages scatter across disk) and doesn't easily support concurrent transactions, which is why log-based WAL recovery dominates in practice.

---

# 15. Concurrency Control — Advanced

*(Builds directly on Section 8's basic 2PL.)*

## 15.1 Strict and Rigorous 2PL

Recall basic 2PL only guarantees serializability, not the avoidance of cascading rollback (Section 13.2), because locks *can* be released before commit, during the shrinking phase while the transaction is still active.

- **Strict 2PL**: all **exclusive (write)** locks held by a transaction are released **only after it commits or aborts** (never earlier). Shared locks may still be released earlier. This guarantees the schedule is **strict** (Section 13.2) — no other transaction can ever read or overwrite an uncommitted write — eliminating cascading rollback for writes. This is the most widely used variant in real systems.
- **Rigorous 2PL**: goes further — **all** locks (both shared and exclusive) are released only after commit/abort. Slightly simpler to implement (one uniform release point) at the cost of marginally less concurrency than strict 2PL, since read locks are also held longer than strictly necessary.

## 15.2 Timestamp Ordering (TO)

An alternative to locking entirely — instead of blocking transactions with locks, assign every transaction a unique **timestamp** at start (based on system clock or a logical counter), and use it to enforce a serialization order **equivalent to the order transactions were timestamped**, without ever actually blocking.

- Each data item tracks a `read_TS` (timestamp of the last transaction that read it) and `write_TS` (timestamp of the last transaction that wrote it).
- Before any read/write, the protocol checks: would this operation violate timestamp order (i.e., is an "older" transaction trying to act on an item after a "younger" transaction already touched it)? If so, the operation is **rejected and the transaction is rolled back and restarted** with a new (later) timestamp, rather than making it wait.
- **Trade-off vs. locking**: timestamp ordering avoids deadlock entirely (no waiting = no wait-for cycles possible) but can suffer more rollbacks/restarts under high contention, since conflicts are resolved by aborting rather than waiting.

## 15.3 Multiversion Concurrency Control (MVCC)

Instead of overwriting a data item on every update (as locking-based schemes do), MVCC keeps **multiple versions** of each item, each tagged with the timestamp (or transaction ID) of the write that created it.

- A read request is given the version that was the most recently committed **as of** the reading transaction's start time — so readers **never block writers, and writers never block readers** (this is the headline benefit).
- Write-write conflicts (two transactions trying to update the same item) still need to be resolved (typically via an abort-and-retry or first-committer-wins rule).
- **Real-world importance**: this is exactly how **PostgreSQL** (and Oracle, and MySQL/InnoDB's default engine) implement much of their concurrency control in practice — a fact worth knowing by name, since "which real DB uses MVCC" is a genuinely common interview follow-up to the isolation-levels question in Section 13.4.

---

# 16. Storage & Disk Management

## 16.1 Disk vs. Memory Storage — the fundamental asymmetry

The entire design of database storage systems is driven by one fact: **disk (secondary storage) I/O is orders of magnitude slower than memory access** (even on SSDs, though the gap is smaller than with spinning magnetic disks). Since data on disk is read/written in fixed-size chunks — **blocks/pages** (commonly 4KB–16KB) — not individual bytes, minimizing the *number of block transfers* is the central optimization goal behind indexing (Section 10), buffer management (below), and query optimization (Section 11).

## 16.2 Buffer Management

The **buffer manager** maintains an in-memory cache (the **buffer pool**) of recently/frequently used disk blocks, so repeated access to the same data doesn't require repeated disk I/O. When a requested block isn't already in the buffer pool (a **buffer miss**), it must be fetched from disk — and if the pool is full, some existing block must be **evicted** first, guided by a **page replacement policy**.

## 16.3 Page Replacement Policies

| Policy | Strategy |
|---|---|
| **LRU (Least Recently Used)** | Evict the block that hasn't been accessed for the longest time — assumes recent access predicts near-future access |
| **MRU (Most Recently Used)** | Evict the block accessed most recently — occasionally useful for specific access patterns like a full sequential scan, where the just-used block is *least* likely to be needed again soon |
| **Clock (Second-Chance)** | An efficient approximation of LRU, using a circular buffer and a per-page "referenced" bit, avoiding the overhead of maintaining exact recency order |

If a block being evicted was modified (**dirty**) while in the buffer, it must be **flushed** (written back) to disk before eviction — otherwise the change would simply be lost, which directly ties back into WAL/recovery (Section 14): the WAL rule requires the *log* record to hit disk before the *dirty data page* itself is flushed.

## 16.4 Record Storage Formats

- **Fixed-length records**: every record occupies the same number of bytes — simple to compute a record's disk address directly by arithmetic (`address = base + record_number × record_size`), but wastes space for variable-length fields (e.g., a `VARCHAR` padded to its max length).
- **Variable-length records**: more space-efficient, but require extra bookkeeping (e.g., a small "slot directory" per block, storing each record's offset and length) since records can't be located by simple arithmetic alone.
- **Spanned vs. unspanned records**: whether a single record is permitted to span across two disk blocks (spanned — more space-efficient but more complex to manage) or must be forced entirely within one block (unspanned — simpler, but can waste space at block boundaries).

---

# 17. Distributed Databases

## 17.1 Why Distribute?

A **distributed database** is a single logical database whose data is physically stored across multiple, possibly geographically separated, machines. Motivations: fault tolerance (no single point of failure), scalability (spread load across machines), and locality (keep data close to the users who most need it, reducing latency).

## 17.2 Data Fragmentation

Splitting a relation into pieces (**fragments**) distributed across different sites:
- **Horizontal fragmentation**: split by **rows** — e.g., European customers on one server, Asian customers on another (this is conceptually the same idea as "sharding" in Section 9.3, just under the classical distributed-DB name).
- **Vertical fragmentation**: split by **columns** — e.g., frequently-accessed customer info on one server, rarely-accessed audit-log columns on another.
- **Mixed (hybrid) fragmentation**: a combination of both.

## 17.3 Data Replication (recap in the distributed-DB context)

As in Section 9.3 — keeping copies of the same data at multiple sites for availability and read performance, at the cost of needing to keep those copies **consistent**, which is precisely what the next two concepts address.

## 17.4 Distributed Transactions — Two-Phase Commit (2PC)

When a single transaction touches data across **multiple** sites, all participating sites must agree to either *all* commit or *all* abort — a single site independently committing while another aborts would break atomicity across the whole distributed system. **Two-Phase Commit** solves this with a designated **coordinator**:

1. **Phase 1 — Voting/Prepare**: the coordinator asks every participating site "are you ready to commit?" Each site does everything needed to guarantee it *can* commit if told to (writes its own local log records), then replies **YES** (ready) or **NO** (abort).
2. **Phase 2 — Decision**: if **all** sites voted YES, the coordinator broadcasts **COMMIT**, and every site commits locally. If **any** site voted NO (or failed to respond), the coordinator broadcasts **ABORT**, and every site rolls back.

2PC guarantees atomicity across sites, but has a well-known weakness: it's a **blocking protocol** — if the coordinator crashes after collecting votes but before broadcasting the final decision, participating sites that already voted YES must simply **wait** (they can't safely decide on their own whether to commit or abort) until the coordinator recovers. (Three-Phase Commit exists to address this blocking weakness, at the cost of extra message rounds — worth knowing exists, rarely required in depth for an intro course.)

## 17.5 CAP Theorem — extremely important for system design + DBMS interviews

In a distributed data system, in the presence of a **network partition** (a communication break between nodes), you can only guarantee **at most two** of the following three properties simultaneously:

- **Consistency (C)**: every read receives the most recent write (or an error) — all nodes see the same data at the same time.
- **Availability (A)**: every request receives a (non-error) response — though not necessarily reflecting the most recent write.
- **Partition tolerance (P)**: the system continues to operate despite arbitrary message loss/delay between nodes.

Because real networks *do* experience partitions, **P is effectively mandatory** for any real distributed system — so the practical trade-off CAP forces is really between **C and A** *when a partition is actually happening*:
- **CP systems**: during a partition, refuse to serve some requests (return an error/timeout) rather than risk returning stale/inconsistent data. *Example*: HBase, MongoDB (in certain configurations), most systems demanding strict consistency.
- **AP systems**: during a partition, keep serving requests from every reachable node, even if that means different nodes might temporarily disagree — resolved later via **eventual consistency** (Section 19). *Example*: Cassandra, DynamoDB, Riak.

Common interview trap to avoid: CAP is specifically about behavior **during a network partition** — it does *not* mean "pick any 2 of 3, always" as a blanket description of a system's everyday behavior when the network is healthy.

---

# 18. Data Warehousing & OLAP

## 18.1 OLTP vs. OLAP

| | **OLTP** (Online Transaction Processing) | **OLAP** (Online Analytical Processing) |
|---|---|---|
| **Purpose** | Day-to-day operational transactions | Historical analysis, business intelligence |
| **Query type** | Many short, simple queries (read/write single rows) | Few, complex, long-running queries (aggregations over huge history) |
| **Data** | Current, highly normalized (to avoid update anomalies — Section 6) | Historical, often intentionally denormalized (for read/aggregation speed) |
| **Example** | A bank processing an ATM withdrawal | A bank analyzing 5 years of transaction trends by region |

A **data warehouse** is a separate, large-scale repository specifically built for OLAP — periodically loaded from OLTP source systems via an **ETL** process, so heavy analytical queries never compete with (or slow down) live transactional workloads.

## 18.2 Star Schema and Snowflake Schema

The dominant data-warehouse design pattern, built around one central **fact table** (numeric, measurable events — e.g., `Sales(Date_id, Product_id, Store_id, Quantity, Revenue)`) surrounded by **dimension tables** (descriptive context — `Date`, `Product`, `Store`).

- **Star schema**: dimension tables are **denormalized** — each is a single flat table (e.g., `Product` includes category name directly, not split into a separate `Category` table). Fewer joins needed per query → faster analytical queries, at the cost of some redundancy within dimension tables (which is fine here — dimension tables are typically small relative to the fact table, so the normalization-driven anomaly concerns from Section 6 matter much less).
- **Snowflake schema**: dimension tables **are** further normalized into sub-dimension tables (e.g., `Product → Category` split out). Saves storage/reduces redundancy, at the cost of more joins per query.

## 18.3 Data Cubes

A conceptual (and sometimes literal, precomputed) multi-dimensional structure — e.g., Sales by `(Product × Region × Time)` — enabling fast **OLAP operations**:
- **Roll-up**: aggregate to a coarser level (daily sales → monthly sales).
- **Drill-down**: the reverse — go to finer detail (yearly → quarterly → monthly).
- **Slice**: fix one dimension to a single value (e.g., only "2025" sales).
- **Dice**: select a sub-cube by filtering on multiple dimensions at once.
- **Pivot (rotate)**: reorient the cube's axes for a different viewing perspective.

## 18.4 ETL Process

**Extract, Transform, Load** — the pipeline that populates a data warehouse from one or more (often heterogeneous) operational source systems:
1. **Extract**: pull raw data out of source systems (OLTP databases, logs, external feeds).
2. **Transform**: clean, validate, restructure, and conform the data to the warehouse's schema (e.g., resolve inconsistent formats, deduplicate, compute derived fields).
3. **Load**: write the transformed data into the warehouse's fact/dimension tables, typically on a scheduled batch basis (e.g., nightly).

---

# 19. NoSQL — Deeper Dive

*(Builds on Section 9's introduction.)*

## 19.1 The Four Main Types, Revisited with When-to-Use Guidance

| Type | Example systems | Best suited for |
|---|---|---|
| **Key-Value** | Redis, DynamoDB | Caching, session storage, simple lookups by a single known key — where you never need to query *inside* the value |
| **Document** | MongoDB, CouchDB | Semi-structured data with a natural "single-entity" shape (a user profile, a product catalog entry) queried by various internal fields, where schema may evolve per-document |
| **Column-family** | Cassandra, HBase | Massive write-heavy workloads, time-series data, cases needing tunable consistency and very high write throughput across huge clusters |
| **Graph** | Neo4j | Highly connected data where the *relationships themselves* are the primary thing being queried — social networks, fraud-detection networks, recommendation engines |

## 19.2 SQL vs. NoSQL — When to Use Which

**Favor a relational (SQL) database when**: the schema is well-understood and relatively stable; strong (ACID) consistency and complex multi-table transactions genuinely matter (e.g., financial ledgers); data is naturally relational and you'll frequently need ad-hoc joins across entities.

**Favor a NoSQL database when**: the data model is naturally document/key-value/graph shaped and doesn't decompose well into normalized tables; the schema needs to evolve rapidly/frequently, or varies per-record; you need to scale writes horizontally beyond what a single relational server can handle; and you can tolerate (or specifically want) eventual, rather than immediate, consistency in exchange for higher availability (recall CAP, Section 17.5).

This isn't a strict either/or in modern practice — many real production systems are **polyglot persistence**: relational for the transactional core, plus one or more NoSQL stores for specific workloads (caching in Redis, full-text/log search in Elasticsearch, etc.) — worth mentioning if an interviewer asks "so which one is *better*."

## 19.3 Eventual Consistency

A consistency model (common in AP-style NoSQL systems — see CAP, Section 17.5) that guarantees: **if no new updates are made to a given data item, all replicas will eventually converge to the same value** — but at any given moment, different replicas might temporarily disagree, and there's no strict guarantee on *how long* "eventually" takes. This is a deliberate trade of strict consistency for higher availability and lower write latency, appropriate when an application can tolerate briefly stale reads (e.g., a social media "like" count that's a few seconds behind is generally fine; a bank balance usually is not).

---

# 20. Security & Authorization

## 20.1 Authentication vs. Authorization — a distinction interviewers love to probe

- **Authentication**: verifying **who** you are (e.g., checking a username/password, a token, biometrics). Answers: "Are you really who you claim to be?"
- **Authorization**: determining **what** an already-authenticated user is *allowed to do* (e.g., can this user read table X? Can they update it? Can they grant others access to it?). Answers: "Now that I know who you are, what are you permitted to do?"

Authentication always logically comes first — you can't meaningfully authorize an unverified identity.

## 20.2 GRANT and REVOKE

SQL's **Data Control Language (DCL)** for managing authorization:
```sql
GRANT SELECT, INSERT ON EMPLOYEE TO analyst_role;
GRANT SELECT ON EMPLOYEE TO bob WITH GRANT OPTION;   -- bob can grant this privilege on to others
REVOKE INSERT ON EMPLOYEE FROM analyst_role;
```
Real systems typically implement **role-based access control (RBAC)**: privileges are granted to *roles* (e.g., `analyst_role`, `admin_role`) rather than directly to individual users, and users are then assigned to roles — this makes managing permissions for large user populations far more tractable than managing every user's privileges individually.

## 20.3 SQL Injection — the classic security vulnerability

Occurs when untrusted user input is directly concatenated into a SQL query string, letting an attacker inject their own SQL logic.

```sql
-- Vulnerable: if username input is:  ' OR '1'='1
SELECT * FROM USERS WHERE username = '" + userInput + "' AND password = '...'
-- becomes: SELECT * FROM USERS WHERE username = '' OR '1'='1' AND password = '...'
-- '1'='1' is always true, potentially bypassing authentication entirely
```

**Defense**: always use **parameterized queries / prepared statements** (where user input is passed as a bound parameter, never concatenated as raw text into the query string), plus input validation and the principle of least privilege for the DB account the application connects with.

---

# 21. Triggers, Views & Constraints — Practical Depth

*(Builds on the brief mentions in Sections 5 and 12.)*

## 21.1 CHECK Constraints — practical patterns

```sql
CREATE TABLE ACCOUNT (
    Balance DECIMAL(12,2) CHECK (Balance >= 0),
    Acc_type VARCHAR(10) CHECK (Acc_type IN ('SAVINGS', 'CURRENT'))
);
```
A `CHECK` constraint is evaluated on every `INSERT`/`UPDATE` to that row; if it evaluates to `FALSE` the statement is rejected (recall from Section 5.3: `UNKNOWN` — e.g., from a NULL comparison — does **not** trigger rejection, only an explicit `FALSE` does).

## 21.2 Assertions

Whereas a `CHECK` constraint is tied to a *single row* of a *single table*, an **assertion** is a schema-level constraint that can span **multiple tables** — e.g., "the total of all employees' salaries in a department must never exceed that department's budget." (Note: full, general assertions are part of the SQL standard but are **not well-supported** in most production databases due to the cost of continuously re-checking them — in practice, the same effect is usually achieved with triggers instead.)

## 21.3 Updatable Views

A view is **updatable** (you can `INSERT`/`UPDATE`/`DELETE` through it, and have those changes correctly reflected in the underlying base table) only under fairly restrictive conditions — generally: it must be based on a **single** base table (no joins), include the base table's primary key, and must not use `DISTINCT`, `GROUP BY`, aggregate functions, or set operations. A view violating these is **read-only**.

## 21.4 Triggers — full syntax pattern

```sql
CREATE TRIGGER trg_salary_audit
AFTER UPDATE OF Salary ON EMPLOYEE
FOR EACH ROW
BEGIN
    INSERT INTO SALARY_AUDIT (Ssn, Old_salary, New_salary, Changed_at)
    VALUES (OLD.Ssn, OLD.Salary, NEW.Salary, CURRENT_TIMESTAMP);
END;
```
- **Timing**: `BEFORE` (validate/modify data before it's written) or `AFTER` (react once the change is already committed to the row, e.g., logging).
- **Event**: `INSERT`, `UPDATE`, or `DELETE` (or a combination).
- **Granularity**: `FOR EACH ROW` (fires once per affected row, with `OLD`/`NEW` row values available) vs. a statement-level trigger (fires once per statement, regardless of how many rows it affected).
- **Common uses**: auditing/history logging (as above), enforcing complex business rules that `CHECK` can't express, automatically maintaining derived/denormalized values, cascading custom application-level logic that plain `ON DELETE CASCADE` can't express.

---

## Closing Notes — How to Actually Use This Document

- **Units I–III** (Sections 1–6) are conceptual/design-heavy — ER modeling and normalization especially reward *practicing by hand* on paper: draw ER diagrams, compute attribute closures, and normalize sample relations yourself rather than just reading the definitions.
- **Units IV–V** (Sections 7–9) plus the transaction/concurrency deep-dives (13, 15) are where "trace through this schedule and tell me if it's serializable / has a deadlock / what isolation level prevents X" style problems live — again, practice by hand with small examples.
- **Sections 10–21** are the material most likely to separate "passed the course" from "did well in interviews" — indexing (B+ Trees), isolation levels, CAP theorem, and window functions come up disproportionately often relative to their coursework weight.

Good luck with it — if any single topic above needs to go deeper (e.g., working through a full normalization example step by step, or tracing a specific schedule for serializability), just ask and we can work through it together rather than just reading more prose.
