# Deploying Letta on Railway

This guide explains how to deploy Letta on Railway, including PostgreSQL with pgvector extensions and PgAdmin for database management.

## Prerequisites

1. A [Railway account](https://railway.app)
2. Railway CLI installed (`npm i -g @railway/cli`)
3. Git repository for your Letta project

## Deployment Steps

### 1. Login to Railway CLI

```bash
railway login
```

### 2. Initialize Railway Project

```bash
# Create a new project
railway init

# Or link to an existing project
railway link
```

### 3. Deploy the Project

```bash
# Deploy with configuration
railway up
```

This will deploy the following services:
- **letta_db**: PostgreSQL with pgvector extensions
- **pgadmin**: Web interface for PostgreSQL management
- **letta_server**: The main Letta application
- **letta_nginx**: Nginx for routing requests

### 4. Configure Environment Variables

Set the following environment variables in Railway dashboard:

```
LETTA_PG_USER=letta
LETTA_PG_PASSWORD=your_secure_password
LETTA_PG_DB=letta
PGADMIN_DEFAULT_EMAIL=your_email@example.com
PGADMIN_DEFAULT_PASSWORD=your_secure_password
OPENAI_API_KEY=your_openai_api_key
# Add any other required API keys
```

## Accessing Services

### PostgreSQL

The PostgreSQL database is accessible:
- **From within Railway**: Use `letta-db:5432` as the host
- **From outside Railway**: Use the public URL from Railway dashboard
- **Connection string**: `postgresql://letta:your_password@hostname:port/letta`

### PgAdmin

Access PgAdmin through the URL provided in the Railway dashboard:
1. Login with your configured email and password
2. Add a new server connection:
   - Host: `letta-db` (internal) or the Railway provided PostgreSQL URL (external)
   - Port: `5432`
   - Username: `letta` (or your configured LETTA_PG_USER)
   - Password: Your configured LETTA_PG_PASSWORD

### Letta Application

The main application is accessible through the Railway-provided URL.

## Monitoring and Logs

View logs and monitor your application:

```bash
railway logs
```

## Updating Your Deployment

After making changes to your code:

```bash
git add .
git commit -m "Update application"
git push

# Deploy the updates
railway up
```

## Troubleshooting

- **Database Connection Issues**: Verify PostgreSQL credentials and check if the service is running
- **PgAdmin Access Problems**: Confirm PgAdmin environment variables are set correctly
- **Application Errors**: Check application logs with `railway logs` 