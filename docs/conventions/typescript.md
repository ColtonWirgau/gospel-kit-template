# TypeScript Conventions

Standards for TypeScript code across the project.

## General Principles

- Use TypeScript strictly - avoid `any` unless absolutely necessary
- Prefer explicit types over inference for function parameters and return types
- Use interfaces for object shapes, types for unions/primitives

## Naming

### Variables and Functions
- `camelCase` for variables and functions: `userName`, `fetchGroups`
- Prefix booleans with `is`, `has`, `should`: `isLoading`, `hasAccess`
- Use descriptive names - avoid abbreviations

### Types and Interfaces
- `PascalCase` for types and interfaces: `Person`, `GroupMember`
- Prefix interfaces with `I` only if needed to avoid conflicts: `IProps` when `Props` is ambiguous
- Suffix type definitions appropriately: `PersonResponse`, `GroupCreateInput`

### Constants
- `SCREAMING_SNAKE_CASE` for true constants: `MAX_RETRY_COUNT`, `API_BASE_URL`
- `camelCase` for const references that could theoretically change

### Files
- `kebab-case` for file names: `group-service.ts`, `use-auth.ts`
- Match component name to file: `PersonCard.tsx` contains `PersonCard`
- Test files: `*.test.ts` or `*.spec.ts`

## Code Style

### Functions
```typescript
// Prefer arrow functions for callbacks and simple functions
const add = (a: number, b: number): number => a + b;

// Use function declarations for complex/hoisted functions
function processRegistration(data: RegistrationInput): Promise<Registration> {
  // ...
}
```

### Async/Await
- Prefer `async/await` over `.then()` chains
- Always handle errors with try/catch or error boundaries
- Don't forget to `await` async functions

### Imports
- Group imports: external libs, then internal modules, then relative imports
- Use absolute imports from `@/` for shared code
- Avoid default exports except for pages/components that require them

```typescript
// External
import { useState, useEffect } from 'react';
import { z } from 'zod';

// Internal absolute
import { db } from '@/lib/db';
import { auth } from '@/lib/auth';

// Relative
import { PersonCard } from './PersonCard';
import type { Person } from './types';
```

### Types

```typescript
// Define types close to where they're used
interface Person {
  id: string;
  firstName: string;
  lastName: string;
  email: string;
  isActive: boolean;
}

// Use Zod for runtime validation
const personSchema = z.object({
  id: z.string(),
  firstName: z.string().min(1),
  lastName: z.string().min(1),
  email: z.string().email(),
  isActive: z.boolean(),
});

type Person = z.infer<typeof personSchema>;
```

## React Specific

### Components
- One component per file (with small helpers allowed)
- Use functional components with hooks
- Destructure props in function signature

```typescript
interface PersonCardProps {
  person: Person;
  onSelect?: (id: string) => void;
}

export function PersonCard({ person, onSelect }: PersonCardProps) {
  return (
    // ...
  );
}
```

### Hooks
- Prefix custom hooks with `use`: `useAuth`, `useGroups`
- Keep hooks focused on a single concern
- Extract complex state logic into custom hooks

### State
- Use `useState` for simple local state
- Use `useReducer` for complex state logic
- Lift state up only when necessary

## Error Handling

- Use typed errors when possible
- Provide meaningful error messages
- Log errors with context for debugging

```typescript
class ApiError extends Error {
  constructor(
    message: string,
    public statusCode: number,
    public code: string
  ) {
    super(message);
    this.name = 'ApiError';
  }
}
```

## Comments

- Don't comment obvious code
- Do comment *why*, not *what*
- Use JSDoc for public APIs and exported functions

```typescript
/**
 * Fetches group members with their attendance records.
 * Excludes inactive members unless explicitly requested.
 */
export async function getGroupMembers(
  groupId: string,
  includeInactive = false
): Promise<GroupMember[]> {
  // ...
}
```
