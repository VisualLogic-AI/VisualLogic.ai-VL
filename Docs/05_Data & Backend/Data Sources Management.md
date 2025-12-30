# Data Sources Management

Data Sources define the **physical storage layer** of the VisualLogic backend.

They represent real databases, tables, fields, and relationships.
 All backend logic ultimately relies on data sources as the **source of truth**, but data sources themselves do not contain business logic.

Understanding how to correctly configure and manage data sources is essential for building stable backend systems and reusable database views.

------

## Role of Data Sources in VisualLogic

In the VisualLogic backend architecture, a data source is responsible for:

- Defining database connections
- Managing table structures and fields
- Describing relationships between tables
- Providing physical data for higher-level logic

A data source answers the question:

> *“Where is the data stored, and how is it structured?”*

It does **not** answer:

- How data is queried
- How business rules are applied
- How frontend applications use the data

Those responsibilities belong to **Database Views** and **Backend Services**.

------

## Creating a Data Source

Data Sources are created and managed in the **backend context** of the IDE.

When creating a data source, you configure:

- **Database type**
   Supported engines typically include MySQL and PostgreSQL.
- **Database scope**
  - Application-level tables
  - Account-level tables
- **Connection configuration**
   Database connection details required by the runtime environment.

Once created, the data source becomes available for binding by Database Views.

------

## Managing Tables and Fields

### Defining Tables

Each data source contains one or more tables.
 For each table, you define:

- Table name
- Field list
- Primary key
- Indexes (when applicable)

Tables represent **physical storage**, not logical data models.

------

### Managing Fields

For each field in a table, you configure:

- Field name
- Data type
- Description
- Required or optional status
- Additional attributes supported by the database engine

Field definitions here determine:

- How data is stored
- What constraints exist at the database level
- Which fields can participate in relationships

Good field definitions at this stage reduce errors and rework later.

------

## Configuring Table Relationships

One of the most important responsibilities of data sources is **defining relationships between tables**.

VisualLogic supports explicit relationship configuration without writing SQL.

------

### Supported Relationship Types

Common relationship types include:

- **One-to-One**
   Example: `User` ↔ `UserProfile`
- **One-to-Many**
   Example: `Class` → `Student`
- **Many-to-One**
   Example: `Order` → `Customer`
- **Many-to-Many**
   Example: `Student` ↔ `Course`

Each relationship is defined by:

1. Selecting the local table and field
2. Selecting the target table and field
3. Choosing the relationship type

------

### Why Relationships Matter

Table relationships enable:

- Joined data access in Database Views
- Field traversal across tables
- Logical field mapping without duplicating schema

Without correctly configured relationships, Database Views cannot expose related data cleanly.

------

## Application Tables and Account Tables

VisualLogic distinguishes between different table scopes.

### Application Tables

- Belong to a specific application
- Store business-specific data
- Typically bound to Database Views and Services

### Account Tables

- Shared across applications
- Store user or account-level data
- Often referenced by multiple backend services

This separation supports multi-application architectures and shared user systems.

------

## Data Import and Export

VisualLogic provides basic data import and export features at the **data source level**.

These features are intended for:

- Initial data setup
- Data migration between environments
- Inspection and debugging

Important characteristics:

- Operates on **physical tables**
- Not part of runtime backend logic
- Should not replace backend services for data manipulation

Database Views and Services should never rely on manual import/export as part of application behavior.

------

## Data Sources as the Physical Layer

It is important to keep data sources **stable and conservative**.

Best practices include:

- Avoid frequent structural changes once services depend on a table
- Use Database Views to absorb schema changes
- Treat data sources as long-lived infrastructure

If changes are required, Database Views act as a buffer between physical schema and application logic.

------

## Common Mistakes to Avoid

- Writing business logic directly against data sources
- Exposing raw data sources to frontend applications
- Encoding business rules into table structure
- Skipping relationship configuration and relying on manual joins later

These practices lead to tightly coupled systems that are hard to reuse and maintain.

------

## Summary

Data Sources define the **physical reality** of backend data in VisualLogic.

- They manage databases, tables, fields, and relationships
- They do not contain business logic
- They support higher-level abstractions through Database Views

A well-managed data source layer is the foundation for reusable database views and reliable backend services.