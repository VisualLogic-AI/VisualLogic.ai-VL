# Database Fields

Database fields are the **fundamental building blocks** of all backend data structures in VisualLogic.

Every table, database view, and backend service ultimately relies on fields to define **what data exists**, **how it is stored**, and **how it flows through the system**. A clear understanding of fields is essential for building stable, reusable, and scalable backend logic.

------

## What Is a Database Field in VisualLogic

In VisualLogic, a **database field** represents a single unit of data within a table or a logical data structure.

A field defines:

- The **name** of the data element
- The **data type**
- Optional **constraints and metadata**
- How the data can be referenced by views and services

Fields exist at multiple layers:

- **Data Source fields** — physical database columns
- **Database View fields** — logical fields mapped to one or more data sources
- **Service input/output fields** — parameters and return values exposed to frontend applications

Although these fields may appear in different contexts, they all follow the same conceptual rules.

------

## Field Types and Data Semantics

Each field has a **data type**, which determines:

- What values are valid
- How the data can be processed
- How it can be used in expressions, conditions, and services

Common field types include:

- Text / String
- Number (integer, decimal)
- Boolean
- Date and time
- JSON / structured data
- Arrays and object collections

Choosing the correct field type is not just a storage decision — it directly affects:

- Formula Editor capabilities
- Conditional logic
- Data validation behavior
- Frontend binding behavior

------

## Field Metadata and Attributes

Beyond type and name, fields can carry additional metadata that improves clarity and maintainability.

Typical field attributes include:

- **Description**
   Explains the purpose and meaning of the field.
- **Required / Optional**
   Indicates whether the field must have a value.
- **Default Value**
   Used when no explicit value is provided.
- **Constraints**
   Such as uniqueness or value limits (depending on data source capabilities).

These attributes are especially important in collaborative environments, where backend logic is reused across projects or by multiple team members.

------

## Fields Across Backend Layers

Although fields appear in different backend components, their roles differ slightly at each layer.

### Fields in Data Sources

At the data source level, fields define:

- Physical storage structure
- Column types and constraints
- Relationship keys between tables

These fields describe **how data is stored**, not how it is consumed.

------

### Fields in Database Views

In Database Views, fields represent a **logical data model**.

A Database View field can be:

- Directly mapped to a data source field
- Mapped through a relationship to another table
- A computed or derived field

Database View fields allow backend logic to operate on a **stable structure**, even if underlying database schemas change.

------

### Fields in Backend Services

In backend services, fields define:

- Input parameters accepted from frontend applications
- Output data returned to the frontend

Service fields form the **contract** between frontend and backend.
 Changing service fields impacts all frontend applications that depend on them.

------

## Designing Fields for Reusability

Good field design is critical for long-term reuse.

Recommended practices:

- Use **clear, descriptive names**
   Avoid abbreviations unless they are universally understood.
- Avoid encoding meaning into field names
   For example, prefer `status` + documented values over `isApprovedOrRejected`.
- Keep logical meaning stable
   A field reused across projects should not change semantics.
- Design with Database Views in mind
   Fields that are likely to be reused should be defined at the view level, not hard-coded into services.

------

## Field Relationships and Indirect Access

Fields often participate in relationships between tables.

In VisualLogic:

- Relationships are defined at the **Data Source** level
- Fields in Database Views can traverse these relationships
- Joined fields can be exposed without duplicating database schema

This allows a single logical field (for example, `userName`) to be derived from a related table while remaining read-only at the logical layer.

Understanding this distinction prevents common mistakes, such as attempting to update related-table fields through a view that only supports main-table modifications.

------

## Common Pitfalls in Field Design

When designing backend fields, avoid the following:

- Treating Database View fields as physical columns
- Mixing storage concerns with business logic
- Overloading a single field with multiple meanings
- Exposing unnecessary internal fields to frontend services
- Changing field semantics without updating dependent services

These issues often lead to fragile backend logic and difficult maintenance later.

------

## Summary

Database fields define the **shape and meaning of data** throughout the VisualLogic backend.

- Fields exist across data sources, views, and services
- Field design impacts logic, reuse, and frontend integration
- Clear semantics and stable definitions are essential

A solid understanding of database fields lays the foundation for effective data source management, database views, and backend service development.