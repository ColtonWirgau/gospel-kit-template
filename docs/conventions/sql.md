# SQL Conventions

This document covers conventions for database work including tables, stored procedures, and queries.

## Naming Conventions

### Tables
- Use `PascalCase` for table names
- Use singular nouns: `Person`, `Event`, `Group` (not `Persons`, `Events`)
- Junction tables: combine both table names alphabetically: `GroupPerson`, `EventRegistration`

### Columns
- Use `snake_case` for column names: `first_name`, `created_at`
- Primary keys: `id` (auto-incrementing integer or UUID)
- Foreign keys: `<table_name>_id` (e.g., `person_id`, `group_id`)
- Timestamps: `created_at`, `updated_at`, `deleted_at`
- Booleans: prefix with `is_` or `has_`: `is_active`, `has_children`

### Stored Procedures
- Prefix with `api_` for procedures exposed via API
- Use descriptive names: `api_GetPersonGroups`, `api_CreateRegistration`
- Include `_JSON` suffix if returning JSON format

### Indexes
- Name format: `idx_<table>_<columns>`
- Example: `idx_person_email`, `idx_event_start_date`

## Query Standards

### Formatting
```sql
SELECT
    p.id,
    p.first_name,
    p.last_name,
    g.name AS group_name
FROM Person p
INNER JOIN GroupPerson gp ON gp.person_id = p.id
INNER JOIN Group g ON g.id = gp.group_id
WHERE p.is_active = 1
    AND g.is_active = 1
ORDER BY p.last_name, p.first_name;
```

- Keywords in UPPERCASE: `SELECT`, `FROM`, `WHERE`, `JOIN`
- One column per line in SELECT
- Indent JOIN conditions and WHERE clauses
- Use table aliases consistently
- Always qualify column names with table alias

### JOINs
- Prefer `INNER JOIN` over implicit joins
- Always specify join type explicitly
- Put the join condition on the same line or indented below

### WHERE Clauses
- Use parameterized queries - never concatenate user input
- Put each condition on its own line with `AND`/`OR` at the start

## Stored Procedures

### Template
```sql
CREATE PROCEDURE api_ExampleProcedure
    @PersonId INT,
    @IncludeInactive BIT = 0
AS
BEGIN
    SET NOCOUNT ON;

    -- Procedure logic here

END
```

### Parameters
- Use `@PascalCase` for parameter names
- Provide default values where sensible
- Document required vs optional parameters

### Error Handling
- Use TRY/CATCH blocks for operations that could fail
- Return meaningful error messages
- Log errors appropriately

## Migrations

- Each migration should be reversible when possible
- One logical change per migration file
- Test migrations on a copy of production data before deploying

## Performance

- Add indexes for frequently queried columns
- Avoid `SELECT *` - specify columns explicitly
- Use `EXISTS` instead of `COUNT(*)` for existence checks
- Consider query plans for complex queries

## Documentation

- Add comments for complex logic
- Document stored procedure parameters and return values
- Keep a changelog for significant schema changes
