# Naming Conventions

Consistent naming across the entire project.

## Quick Reference

| Context | Convention | Example |
|---------|------------|---------|
| Files (general) | kebab-case | `user-service.ts` |
| React Components | PascalCase | `PersonCard.tsx` |
| Variables/Functions | camelCase | `getUserById` |
| Types/Interfaces | PascalCase | `Person`, `GroupMember` |
| Constants | SCREAMING_SNAKE_CASE | `MAX_RETRY_COUNT` |
| CSS Classes | kebab-case | `card-header` |
| Database Tables | PascalCase | `Person`, `GroupMember` |
| Database Columns | snake_case | `first_name`, `created_at` |
| SQL Parameters | @PascalCase | `@PersonId`, `@GroupName` |
| API URLs | kebab-case | `/api/group-members` |
| Env Variables | SCREAMING_SNAKE_CASE | `DATABASE_URL` |

## Files and Folders

### General Files
```
user-service.ts
auth-utils.ts
api-client.ts
```

### React Components
```
PersonCard.tsx
GroupList.tsx
EventRegistrationForm.tsx
```

### Test Files
```
user-service.test.ts
PersonCard.test.tsx
```

### Folders
```
components/
services/
utils/
group-management/  # feature folders in kebab-case
```

## Code

### Variables
```typescript
// Good
const userName = 'John';
const isActive = true;
const groupMembers = [];

// Bad
const user_name = 'John';
const UserName = 'John';
const active = true;  // unclear if boolean
```

### Functions
```typescript
// Good
function getUserById(id: string) {}
function calculateTotalAttendance() {}
function isValidEmail(email: string) {}

// Bad
function GetUserById(id: string) {}  // not PascalCase
function user_get(id: string) {}     // not snake_case
function process(x: any) {}          // vague name
```

### Booleans
```typescript
// Prefix with is, has, should, can, will
const isLoading = true;
const hasPermission = false;
const shouldRefresh = true;
const canEdit = false;
const willExpire = true;
```

### Arrays and Collections
```typescript
// Use plural nouns
const users = [];
const groupMembers = [];
const activeEvents = [];

// Not
const userList = [];      // redundant 'List'
const userArray = [];     // redundant 'Array'
```

## Types and Interfaces

```typescript
// Types - PascalCase, descriptive
type Person = { ... };
type GroupMember = { ... };
type ApiResponse<T> = { ... };

// Input/Output suffixes when needed
type CreatePersonInput = { ... };
type PersonResponse = { ... };

// Props suffix for React
interface PersonCardProps { ... }
interface GroupListProps { ... }
```

## Constants

```typescript
// True constants - SCREAMING_SNAKE_CASE
const MAX_FILE_SIZE = 5 * 1024 * 1024;
const API_BASE_URL = 'https://api.example.com';
const DEFAULT_PAGE_SIZE = 20;

// Object constants - PascalCase keys
const HttpStatus = {
  Ok: 200,
  Created: 201,
  NotFound: 404,
} as const;
```

## Database

### Tables
```sql
-- PascalCase, singular
Person
Group
EventRegistration
GroupPerson  -- junction table
```

### Columns
```sql
-- snake_case
id
first_name
last_name
created_at
updated_at
is_active
person_id  -- foreign key
```

## API

### URLs
```
/api/groups
/api/group-members
/api/event-registrations
/api/groups/:id/members
```

### JSON Keys
```json
{
  "firstName": "John",
  "lastName": "Doe",
  "groupId": "123",
  "createdAt": "2024-01-15T10:30:00Z"
}
```

## Abbreviations

- Avoid abbreviations unless universally understood
- Common acceptable abbreviations: `id`, `url`, `api`, `db`, `auth`
- When in doubt, spell it out

```typescript
// Good
const userId = '123';
const apiUrl = 'https://...';
const authToken = '...';

// Bad
const usrId = '123';     // unclear abbreviation
const regForm = { ... }; // spell out 'registration'
```
