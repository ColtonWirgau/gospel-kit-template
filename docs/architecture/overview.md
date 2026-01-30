# System Architecture Overview

High-level architecture of the Gospel Kit Template.

## Project Structure

```
gospel-kit-template/
├── apps/                 # Deployable applications
├── packages/             # Shared libraries and utilities
├── database/             # Database schemas and migrations
├── widgets/              # Embeddable widget components
├── microsites/           # Standalone mini-applications
├── templates/            # Reusable templates
└── docs/                 # Documentation (you are here)
```

## Monorepo Structure

This project uses a **Turborepo** monorepo with the following organization:

### Apps
Full applications that get deployed:
- Each app is a self-contained deployable unit
- Apps can depend on packages but not on other apps
- Each app has its own configuration and deployment

### Packages
Shared code used across apps:
- `packages/ui` - Shared UI components
- `packages/config` - Shared configuration
- `packages/db` - Database client and schemas
- Other shared utilities

### Database
Database-related code:
- Schema definitions (Drizzle ORM)
- Migrations
- Seed data

## Technology Stack

| Layer | Technology |
|-------|------------|
| Framework | Next.js 15 |
| Language | TypeScript |
| Styling | Tailwind CSS v4 |
| Database | PostgreSQL via Drizzle ORM |
| Authentication | NextAuth.js v5 |
| Testing | Vitest |
| Build | Turborepo |
| Package Manager | npm |

## Data Flow

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Client    │────▶│   Next.js   │────▶│  Database   │
│  (Browser)  │◀────│   Server    │◀────│ (PostgreSQL)│
└─────────────┘     └─────────────┘     └─────────────┘
                           │
                           ▼
                    ┌─────────────┐
                    │  External   │
                    │    APIs     │
                    └─────────────┘
```

## Key Concepts

### Server Components
- Default for all React components
- Data fetching happens on the server
- No client-side JavaScript unless needed

### Client Components
- Used for interactivity (forms, state)
- Marked with `'use client'` directive
- Keep as small as possible

### API Routes
- Located in `app/api/`
- Handle server-side logic
- Return JSON responses

### Database Access
- Use Drizzle ORM for type-safe queries
- Define schemas in `database/schema/`
- Run migrations before deploying

## Deployment

Each app can be deployed independently:
- Vercel (recommended for Next.js)
- Docker containers
- Any Node.js hosting

## Environment Configuration

See [SETUP.md](../../SETUP.md) for environment variable documentation.

## Further Reading

- [Database Schema](./database.md)
- [API Conventions](../conventions/api.md)
- [TypeScript Conventions](../conventions/typescript.md)
