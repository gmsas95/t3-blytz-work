# Deployment Guide

## Prerequisites
- Docker and Docker Compose installed
- PostgreSQL database (either via Docker or external)
- Environment variables configured

## Environment Variables

Create a `.env` file based on `.env.example`:

```bash
# Required for NextAuth
AUTH_SECRET="your-secret-key-min-32-characters"

# For Google OAuth (optional but recommended)
GOOGLE_CLIENT_ID="your-google-client-id"
GOOGLE_CLIENT_SECRET="your-google-client-secret"

# Database connection
DATABASE_URL="postgresql://user:password@postgres:5432/blytz_work"
```

## Local Development with Docker

1. **Start PostgreSQL and App:**
```bash
docker-compose up -d
```

2. **Run Prisma Migrations:**
```bash
docker-compose exec app npm run db:push
```

3. **Access Application:**
- Frontend: http://localhost:3000
- Prisma Studio: `docker-compose exec app npm run db:studio`

## Production Deployment

### Option 1: Docker Compose

1. **Set Production Environment Variables:**
```bash
export NODE_ENV=production
export POSTGRES_USER=blytz_user
export POSTGRES_PASSWORD=your_secure_password
export POSTGRES_DB=blytz_work
export AUTH_SECRET=your_production_secret
export GOOGLE_CLIENT_ID=your_google_client_id
export GOOGLE_CLIENT_SECRET=your_google_client_secret
```

2. **Build and Start:**
```bash
docker-compose up -d --build
```

### Option 2: Build and Run Docker Image

1. **Build Image:**
```bash
docker build -t t3-blytz-work:latest .
```

2. **Run with Environment Variables:**
```bash
docker run -d \
  -p 3000:3000 \
  -e DATABASE_URL="postgresql://postgres:password@postgres-host:5432/blytz_work" \
  -e AUTH_SECRET="your-secret" \
  -e GOOGLE_CLIENT_ID="your-client-id" \
  -e GOOGLE_CLIENT_SECRET="your-client-secret" \
  -e NODE_ENV=production \
  t3-blytz-work:latest
```

## Database Setup

### Fresh Install
```bash
# Run migrations
npm run db:push

# Or generate and run migrations
npm run db:generate
npm run db:migrate
```

### Reset Database
```bash
# Warning: This will delete all data
npm run db:push --force-reset
```

## Health Checks

### App Health
```bash
curl http://localhost:3000/health
```

### Database Health
```bash
docker-compose exec postgres pg_isready -U blytz_user -d blytz_work
```

## Troubleshooting

### Build Issues
```bash
# Skip environment validation for builds
SKIP_ENV_VALIDATION=1 npm run build
```

### Database Connection Issues
```bash
# Check PostgreSQL logs
docker-compose logs postgres

# Check database connection string
echo $DATABASE_URL
```

### NextAuth Issues
```bash
# Check AUTH_SECRET is set and >= 32 characters
echo $AUTH_SECRET | wc -c

# Verify Google OAuth credentials
echo $GOOGLE_CLIENT_ID
echo $GOOGLE_CLIENT_SECRET
```

## Environment Variables Reference

| Variable | Required | Description | Example |
|-----------|-----------|-------------|----------|
| `AUTH_SECRET` | Yes | Secret key for NextAuth | Generate with `npx auth secret` |
| `DATABASE_URL` | Yes | PostgreSQL connection string | `postgresql://user:pass@host:5432/db` |
| `GOOGLE_CLIENT_ID` | No | Google OAuth Client ID | `123456789.apps.googleusercontent.com` |
| `GOOGLE_CLIENT_SECRET` | No | Google OAuth Client Secret | `GOCSPX-abc123` |
| `NODE_ENV` | No | App environment | `development` or `production` |

## CI/CD Example (GitHub Actions)

```yaml
name: Deploy

on:
  push:
    branches: [master]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Build Docker image
        run: docker build -t t3-blytz-work:latest .

      - name: Push to registry
        run: docker push t3-blytz-work:latest
```

## Monitoring

### View Logs
```bash
# All services
docker-compose logs -f

# Specific service
docker-compose logs -f app
docker-compose logs -f postgres
```

### Resource Usage
```bash
docker stats
```
