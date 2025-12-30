# Database Views

Database Views are the **core abstraction layer** of backend data logic in VisualLogic.

They define *how data is accessed, organized, and reused* without binding backend logic directly to physical database tables. By separating database operations from database structure, Database Views enable reuse, flexibility, and multi-project scalability.

------

## What Is a Database View

A **Database View** is a **pure backend logic component**.

It is **not** a database table and does **not** store data itself.
 Instead, it provides:

- A logical data model
- Encapsulated database operation logic
- A stable interface for backend services
- A mapping layer between logic and physical data sources

A Database View answers the question:

> *“How should this data be accessed and manipulated?”*

------

## Why Database Views Exist

In traditional systems, database logic is often tightly coupled to tables and schemas.
 VisualLogic avoids this by introducing Database Views as a dedicated abstraction layer.

Key benefits include:

### Logic Reuse

The same Database View can be reused across:

- Multiple frontend applications
- Multiple backend services
- Multiple projects

Only the **data source binding** needs to change.

------

### Data Source Replacement

Database Views allow you to:

- Replace the underlying data source in the **Properties Panel**
- Bind the same logic to different databases
- Adapt to schema changes without rewriting logic

This makes Database Views ideal for templates, multi-tenant systems, and evolving projects.

------

### Clear Separation of Responsibility

- **Data Sources** define *where data lives*
- **Database Views** define *how data is structured and accessed*
- **Services** define *how applications use data*

Each layer has a clear boundary.

------

## Database Views and Data Sources

Database Views always operate on **bound data sources**.

A Database View can be bound to:

- One **main data source** (required)
- One or more **related data sources** through configured relationships

The main data source determines:

- Which table can be inserted, updated, or deleted
- Which fields are writable

Related data sources are used for **joined output**, not direct modification.

------

## Recommended Usage Workflows

VisualLogic supports two recommended workflows when working with Database Views.

------

### Workflow 1: Start from Data Sources

Use this approach when:

- A database already exists
- Data relationships must be planned upfront

Steps:

1. Configure data sources and define table fields
2. Configure relationships between tables
3. Create a Database View
4. Bind the Database View to the main data source
5. Select and map required fields
6. Expose operations through backend services

This approach ensures strong control over schema design.

------

### Workflow 2: Start from Database View

Use this approach when:

- Creating a new database
- Rapid prototyping is required

Steps:

1. Create a Database View
2. Design the logical field structure
3. Use **Create DB and Bind** to generate a database table
4. Configure relationships in the data source
5. Adjust field mappings as needed

> Important:
>  Relationship fields are **not automatically configured** during database creation and must be defined manually afterward.

------

## Creating a Database View

### Adding the Component

Database View components are available under:

**Component Panel → Backend Logic → Database View**

To add a Database View:

1. Select the Backend App in the Object Tree
2. Add the Database View component
3. The view appears in the Backend Object Tree

------

### Designing the Field Structure

Database View fields define the **logical data structure**.

You can:

- Add fields using **Add Field**
- Specify field name and data type
- Add descriptive metadata
- Define as many fields as needed

Fields can represent:

- Direct fields from the main table
- Fields from related tables
- Derived or computed values

The field structure defines the contract used by backend services.

------

## Writing Data Operation Logic

Database Views support writing data operation logic directly on the view.

Common operation types include:

- Data query
- Data creation
- Data update
- Data deletion

These operations:

- Act on the **main data source**
- Can include fields from related data sources in output
- Are reusable across multiple services

Database Views are typically used as the **target object** for backend services.

------

## Binding Data Sources

### Binding a Main Data Source

To bind a Database View:

1. Select the Database View
2. Choose a main data source in the Properties Panel
3. The system attempts to auto-match fields

The main data source determines write permissions.

------

### Field Mapping

Each Database View field must be mapped to a data source field.

Mapping options include:

- Direct field mapping
- Relationship-based field mapping

For example:

If a `studentName` field does not exist in the main `Score` table, but a relationship exists to `Student`, you can map:

```
Score.studentId → Student.name
```

This allows joined output without duplicating data.

------

### Create DB and Bind

When starting from a Database View:

1. Design all required fields
2. Click **Create DB and Bind**
3. VisualLogic automatically:
   - Creates a matching database table
   - Binds it as the main data source
   - Maps fields automatically

After creation:

- Table relationships must be configured manually
- Field mappings can be adjusted freely

------

## Viewing and Editing Data

Database View data can be inspected through:

- Preview mode
- Published data table views

Important constraints:

- Manual editing applies **only to the main data source**
- Fields from related tables are **read-only**

Understanding this distinction is critical when debugging or testing data logic.

------

## Reusing Database Views Across Projects

Database Views can be:

- Copied between projects
- Encapsulated in code snippets
- Rebound to different data sources

This enables:

- Rapid project setup
- Consistent backend logic
- Reduced duplication

Database Views are the preferred unit of backend reuse in VisualLogic.

------

## Limitations and Best Practices

### Limitations

- Only the main data source is writable
- Relationship configuration is required for joined fields
- Database Views do not replace backend services

------

### Best Practices

- Keep Database Views focused on data logic
- Avoid embedding business rules in views
- Use services for orchestration and validation
- Design views for reuse, not one-off cases

------

## Summary

Database Views are the **foundation of reusable backend data logic** in VisualLogic.

They:

- Abstract physical database structure
- Encapsulate data access logic
- Enable data source replacement
- Support multi-project and multi-frontend reuse

Mastering Database Views is essential for effective backend development in VisualLogic.