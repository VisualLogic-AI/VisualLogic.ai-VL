# Data and Backend Overview

This section introduces how backend data and services are designed and managed in VisualLogic.

Unlike traditional development environments that mix database access, business logic, and API definitions together, VisualLogic adopts a **layered, component-based backend model**. This model is designed to support reuse, multi-frontend applications, and long-term maintainability.

------

## The Goal of the VisualLogic Backend

The VisualLogic backend is designed to solve three core problems:

1. **Managing data in a structured and reusable way**
2. **Separating database structure from business logic**
3. **Allowing multiple frontend applications to share the same backend capabilities**

In practice, this means backend development in VisualLogic focuses on **composition and abstraction**, not direct database manipulation.

------

## The Backend Layer Model

VisualLogic organizes backend development into three main layers:

```
Data Source → Database View → Backend Service → Frontend Application
```

Each layer has a clear responsibility and should not overlap with others.

------

### Data Sources: Physical Data Storage

**Data Sources** represent the physical database layer.

They define:

- Database connections (e.g. MySQL, PostgreSQL)
- Tables and fields
- Field types and constraints
- Relationships between tables

A Data Source answers the question:

> *“Where is the data stored, and how are tables related?”*

Data Sources describe **what exists**, not **how it is used**.

------

### Database Views: Logical Data Abstraction

**Database Views** are pure backend logic components.

They sit between Data Sources and Services and provide:

- A logical data structure independent of physical tables
- Field selection and mapping (including fields from related tables)
- Encapsulated database operation logic
- The ability to replace underlying data sources without rewriting logic

A Database View answers the question:

> *“How should data be organized and accessed?”*

This layer is the **key abstraction** that makes backend logic reusable across projects.

------

### Backend Services: Application Capabilities

**Backend Services** expose backend functionality to frontend applications.

They are responsible for:

- Defining callable operations
- Accepting parameters from the frontend
- Executing logic based on Database Views
- Returning structured results

A Backend Service answers the question:

> *“What capabilities does the backend provide to applications?”*

Frontend applications never access Data Sources or Database Views directly.
 All communication happens through Services.

------

## Why VisualLogic Separates These Layers

This separation is intentional and critical.

By decoupling physical storage, logical data structure, and application behavior:

- Backend logic becomes **reusable**
- Multiple frontend applications can share the same backend
- Database structure changes do not automatically break business logic
- Projects can evolve without constant refactoring

This design also allows Database Views to be copied, reused, or bound to new data sources in different projects.

------

## Typical Backend Development Flow

While VisualLogic supports flexible workflows, a typical backend development process looks like this:

1. Define or connect **Data Sources**
2. Configure tables, fields, and relationships
3. Create **Database Views** to model logical data structures
4. Bind views to data sources and map fields
5. Implement **Backend Services** based on database views
6. Call services from frontend applications

This flow ensures that data structure, logic, and usage remain clearly separated.

------

## Data Import and Export

VisualLogic provides basic data import and export capabilities at the data source level.

These features are intended for:

- Initial data setup
- Data migration
- Debugging and inspection

They are **not** part of runtime business logic and should not replace backend services.

------

## How to Read the Following Documents

This section is organized from **lowest-level concepts to highest-level behavior**:

1. **Database Fields**
    Learn how fields are defined and how they affect data structure.
2. **Data Sources Management**
    Learn how databases, tables, and relationships are managed.
3. **Database Views**
    Learn how database logic is abstracted, reused, and bound to data sources.
4. **Backend Logic and Services**
    Learn how backend capabilities are exposed to frontend applications.

Reading them in this order is strongly recommended.

------

## Summary

VisualLogic backend development is built on **clarity and separation of responsibility**.

- Data Sources define **where data lives**
- Database Views define **how data is structured and reused**
- Backend Services define **what applications can do**

Understanding this model is essential for building scalable, multi-application systems with VisualLogic.