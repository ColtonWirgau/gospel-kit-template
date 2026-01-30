# API Conventions

Standards for designing and implementing REST APIs.

## URL Structure

### Base Patterns
```
GET    /api/resources          # List resources
POST   /api/resources          # Create resource
GET    /api/resources/:id      # Get single resource
PUT    /api/resources/:id      # Update resource (full)
PATCH  /api/resources/:id      # Update resource (partial)
DELETE /api/resources/:id      # Delete resource
```

### Naming
- Use `kebab-case` for URLs: `/api/group-members`, not `/api/groupMembers`
- Use plural nouns for collections: `/api/groups`, `/api/people`
- Nest related resources logically: `/api/groups/:id/members`

### Query Parameters
- `page` and `limit` for pagination: `?page=1&limit=20`
- `sort` for ordering: `?sort=created_at:desc`
- `filter` or specific params for filtering: `?status=active` or `?filter[status]=active`
- `include` for related resources: `?include=members,events`

## Request/Response Format

### Requests
```typescript
// POST /api/groups
{
  "name": "Young Adults",
  "description": "Weekly gathering for young adults",
  "leaderId": "123"
}
```

- Use `camelCase` for JSON keys
- Send only the fields being created/updated
- Validate input with Zod schemas

### Responses

#### Success (Single Resource)
```typescript
// GET /api/groups/123
{
  "id": "123",
  "name": "Young Adults",
  "description": "Weekly gathering for young adults",
  "leaderId": "456",
  "createdAt": "2024-01-15T10:30:00Z",
  "updatedAt": "2024-01-15T10:30:00Z"
}
```

#### Success (Collection)
```typescript
// GET /api/groups
{
  "data": [
    { "id": "123", "name": "Young Adults", ... },
    { "id": "124", "name": "Men's Group", ... }
  ],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total": 45,
    "totalPages": 3
  }
}
```

#### Errors
```typescript
// 400 Bad Request
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid request data",
    "details": [
      { "field": "email", "message": "Invalid email format" }
    ]
  }
}

// 404 Not Found
{
  "error": {
    "code": "NOT_FOUND",
    "message": "Group not found"
  }
}

// 500 Internal Server Error
{
  "error": {
    "code": "INTERNAL_ERROR",
    "message": "An unexpected error occurred"
  }
}
```

## HTTP Status Codes

| Code | Use Case |
|------|----------|
| 200 | Success (GET, PUT, PATCH) |
| 201 | Created (POST) |
| 204 | No Content (DELETE) |
| 400 | Bad Request (validation errors) |
| 401 | Unauthorized (not authenticated) |
| 403 | Forbidden (not authorized) |
| 404 | Not Found |
| 409 | Conflict (duplicate, etc.) |
| 422 | Unprocessable Entity (business logic error) |
| 500 | Internal Server Error |

## Authentication

- Use Bearer tokens in Authorization header: `Authorization: Bearer <token>`
- Return 401 for missing/invalid tokens
- Return 403 for valid token but insufficient permissions

## Versioning

- Use URL versioning when needed: `/api/v1/groups`
- Avoid breaking changes - add new fields, don't remove
- Deprecate endpoints with headers before removal

## Rate Limiting

- Include rate limit headers in responses:
  - `X-RateLimit-Limit`: requests per window
  - `X-RateLimit-Remaining`: requests left
  - `X-RateLimit-Reset`: timestamp when limit resets
- Return 429 Too Many Requests when exceeded

## Documentation

- Document all endpoints with expected request/response shapes
- Include example requests and responses
- Note required vs optional parameters
- Document error conditions
