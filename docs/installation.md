# Installation

## Prerequisites

- Node.js 18+
- npm
- PostgreSQL (or Neon database)

## Setup

1. Clone the repository
   ```bash
   git clone https://github.com/ColtonWirgau/gospel-kit-template.git
   cd gospel-kit-template
   ```

2. Install dependencies
   ```bash
   npm install
   ```

3. Copy environment variables
   ```bash
   cp .env.example .env.local
   ```

4. Configure your `.env.local` with required values

5. Run database migrations
   ```bash
   npm run db:migrate
   ```

6. Start the development server
   ```bash
   npm run dev
   ```

## Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `DATABASE_URL` | PostgreSQL connection string | Yes |
| `NEXTAUTH_SECRET` | Random string for session encryption | Yes |
| `NEXTAUTH_URL` | Your app URL (http://localhost:3000 for dev) | Yes |

## Troubleshooting

### Common Issues

**Database connection failed**
- Verify your `DATABASE_URL` is correct
- Ensure the database server is running

**Port already in use**
- Kill the process using port 3000 or use a different port
