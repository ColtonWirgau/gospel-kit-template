# Claude Code Skills

Reusable skills for common Gospel Kit development tasks.

## Organization

Skills are organized by category:

### `general/`
Architecture and template skills that work across all databases:
- `/general/new-micro-app` - Create a new micro-app
- `/general/mark-upstream` - Document template bug fixes
- `/general/new-microsite` (future)
- `/general/new-widget` (future)

### `ministry-platform/`
Skills specific to MinistryPlatform integration:
- `/ministry-platform/new-entity` - Create Zod schema for MP table
- `/ministry-platform/new-stored-proc` - Create MP stored procedure
- `/ministry-platform/new-api-route` - Create API route for MP data
- `/ministry-platform/mp-troubleshoot` - Debug MP API issues

### `neon/`
Skills specific to Neon PostgreSQL (coming soon):
- `/neon/new-migration` - Create Neon migration
- `/neon/new-schema` - Create Zod schema for Neon table
- `/neon/neon-troubleshoot` - Debug Neon issues

## Usage

Call skills by their full path or short name:

```bash
# Full path (explicit)
/ministry-platform/new-entity

# Short name (if unique)
/new-entity

# General skills
/new-micro-app
/mark-upstream
```

## Adding New Skills

1. Determine which category the skill belongs to
2. Create a new `.md` file in the appropriate directory
3. Follow the existing skill format with `---description---` frontmatter
4. Test the skill with `/skill-name`

## Skill Categories Guide

**Use `general/` for:**
- Architecture decisions (micro-apps, microsites, widgets)
- Template management (upstream fixes, releases)
- Cross-database operations

**Use `ministry-platform/` for:**
- MP API integration
- MP stored procedures
- MP table schemas
- MP troubleshooting

**Use `neon/` for:**
- Neon migrations
- Neon table schemas
- Neon client configuration
- Neon troubleshooting

**Future categories:**
- `rock-rms/` - Rock RMS integration skills
- `planning-center/` - Planning Center integration skills
