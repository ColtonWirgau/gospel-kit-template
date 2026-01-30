# Database Architecture

Database design principles and schema documentation.

## Overview

The project uses **PostgreSQL** with **Drizzle ORM** for type-safe database access.

## Schema Location

Database schemas are defined in:
```
database/
├── schema/           # Table definitions
├── migrations/       # Generated migrations
└── seed/             # Seed data for development
```

## Core Tables

> **Note:** Update this section as schemas are defined.

### Example Schema Structure

```typescript
// database/schema/person.ts
import { pgTable, text, timestamp, boolean } from 'drizzle-orm/pg-core';

export const person = pgTable('Person', {
  id: text('id').primaryKey(),
  firstName: text('first_name').notNull(),
  lastName: text('last_name').notNull(),
  email: text('email').unique().notNull(),
  isActive: boolean('is_active').default(true),
  createdAt: timestamp('created_at').defaultNow(),
  updatedAt: timestamp('updated_at').defaultNow(),
});
```

## Relationships

### One-to-Many
```typescript
// A group has many members
export const group = pgTable('Group', {
  id: text('id').primaryKey(),
  name: text('name').notNull(),
  leaderId: text('leader_id').references(() => person.id),
});
```

### Many-to-Many
```typescript
// Junction table for group members
export const groupMember = pgTable('GroupMember', {
  groupId: text('group_id').references(() => group.id),
  personId: text('person_id').references(() => person.id),
  role: text('role').default('member'),
  joinedAt: timestamp('joined_at').defaultNow(),
}, (table) => ({
  pk: primaryKey({ columns: [table.groupId, table.personId] }),
}));
```

## Migrations

### Creating Migrations
```bash
# Generate migration from schema changes
npm run db:generate

# Apply migrations
npm run db:migrate

# Push schema directly (development only)
npm run db:push
```

### Migration Best Practices
- One logical change per migration
- Test migrations on a copy of production data
- Never edit a migration after it's been applied to production
- Include both up and down migrations when possible

## Indexes

Add indexes for:
- Foreign key columns
- Columns used in WHERE clauses
- Columns used in ORDER BY
- Columns used in JOIN conditions

```typescript
export const person = pgTable('Person', {
  // ... columns
}, (table) => ({
  emailIdx: index('idx_person_email').on(table.email),
  activeIdx: index('idx_person_is_active').on(table.isActive),
}));
```

## Soft Deletes

For tables that need soft delete:

```typescript
export const person = pgTable('Person', {
  // ... other columns
  deletedAt: timestamp('deleted_at'),
});
```

Query active records:
```typescript
const activeUsers = await db
  .select()
  .from(person)
  .where(isNull(person.deletedAt));
```

## Timestamps

All tables should include:
- `created_at` - Set on insert, never updated
- `updated_at` - Updated on every modification

```typescript
import { timestamp } from 'drizzle-orm/pg-core';

const timestamps = {
  createdAt: timestamp('created_at').defaultNow().notNull(),
  updatedAt: timestamp('updated_at').defaultNow().notNull(),
};
```

## Naming Conventions

See [SQL Conventions](../conventions/sql.md) for detailed naming rules.

| Element | Convention | Example |
|---------|------------|---------|
| Tables | PascalCase, singular | `Person`, `Group` |
| Columns | snake_case | `first_name`, `created_at` |
| Indexes | idx_table_column | `idx_person_email` |
| Foreign Keys | table_id | `person_id`, `group_id` |

## Security

- Never store plaintext passwords
- Use parameterized queries (Drizzle handles this)
- Limit database user permissions
- Encrypt sensitive data at rest
- Audit access to sensitive tables
