# Deploying Letta on Railway

This guide explains how to deploy Letta on Railway using the same approach as the official template, with optional PgAdmin for database management.

## Deployment Options

You have two options for deploying Letta on Railway:

### Option 1: Use the Official Letta Template (Recommended)

The simplest way to deploy Letta is to use the official Railway template:

1. Go to https://railway.com/template/jgUR1t?referralCode=kdR8zc
2. Click "Deploy Now"
3. Fill in the required environment variables

The official template provides:
- Latest Letta Docker image
- PostgreSQL database with pgvector
- Pre-configured environment variables

### Option 2: Custom Deployment with PgAdmin (Advanced)

If you need a custom setup with PgAdmin and more control over the configuration, follow the steps below.

## Prerequisites

1. A [Railway account](https://railway.app)
2. Railway CLI installed (`npm i -g @railway/cli`)
3. Git repository for your Letta project

## Custom Deployment Steps

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

When you run the deployment command, Railway will prompt you for required variables:

```bash
railway up
```

You'll be asked to provide the following information:
- **PostgreSQL Username**: Database user (default: letta)
- **PostgreSQL Password**: Secure password for database access
- **PostgreSQL Database Name**: Name of the database (default: letta)
- **Letta Server Password**: Password for connecting from app.letta.com
- **LLM API Keys**: Various API keys for LLM providers (OpenAI, Anthropic, etc.)
- **PgAdmin Email & Password**: If using our custom deployment with PgAdmin

These values will be securely stored in Railway's environment and used during deployment.

This will deploy the following services:
- **letta_db**: PostgreSQL with pgvector extensions
- **letta_server**: The main Letta application
- **pgadmin**: (Optional) Web interface for PostgreSQL management

## Volumes and Data Persistence

Railway manages persistent volumes for your services, ensuring your data is preserved across deployments and service restarts.

### PostgreSQL Data Storage

Our setup uses the exact same data storage approach as the official template:

- **Volume name**: `postgresql`
- **Railway storage path**: `/data/postgresql` 
  (This is the same path used in the official Letta template)
- **Container mount point**: `/var/lib/postgresql/data`
- **Purpose**: Stores all PostgreSQL database files, tables, indexes, and vector data

When you deploy using our configuration or the official template, Railway creates this persistent volume and mounts it to the PostgreSQL container. This ensures your data persists even if containers restart or are redeployed.

### PgAdmin Data (Custom Setup Only)

If using our custom setup with PgAdmin:

- **Volume name**: `pgadmin_data`
- **Container mount point**: `/var/lib/pgadmin`
- **Purpose**: Stores PgAdmin configuration and connection information

### How Railway Manages Volumes

Railway's volume system provides:
- Data persistence across container restarts and redeployments
- Automatic infrastructure-level backups
- Region-specific storage (in the same region as your deployment)

Important notes about Railway volumes:
- They are managed automatically by Railway's infrastructure
- They follow Railway's retention and backup policies
- For critical data, it's recommended to set up additional backup strategies

## Connecting to Letta

After deployment, you can connect to your Letta server:

1. Go to [https://app.letta.com](https://app.letta.com)
2. Connect to your remote development server:
   - Use the Letta server's public domain from Railway (e.g., https://your-project-name.railway.app:8083)
   - Enter the Letta Server Password you configured during deployment

## Supported LLM Providers

As in the official template, Letta supports multiple LLM providers:

1. **OpenAI**: Connect to GPT models with `OPENAI_API_KEY`
2. **Anthropic**: Connect to Claude models with `ANTHROPIC_API_KEY`
3. **Composio**: Access 7k+ pre-made tools with `COMPOSIO_API_KEY`

Additional providers available:
4. **Groq**: Set `GROQ_API_KEY`
5. **Azure OpenAI**: Set `AZURE_API_KEY`, `AZURE_BASE_URL`, and `AZURE_API_VERSION`
6. **Google Gemini**: Set `GEMINI_API_KEY`

For details on all available providers, see the [Letta documentation](https://docs.letta.com/guides/server/docker).

## Accessing Services

### Letta Server

The main Letta server is accessible at the Railway-generated domain:
- HTTP API: Port 8083
- WebSocket: Port 8283

Connect from app.letta.com using this domain and the password you configured.

### PostgreSQL Database

The PostgreSQL database is accessible:
- **From within Railway**: Using the private domain automatically assigned by Railway
- **From outside Railway**: Using the public domain if you've enabled external access
- **Connection string**: `postgresql://letta:your_password@hostname:port/letta`

### PgAdmin (if included)

If you're using our custom setup with PgAdmin:
1. Access PgAdmin through the public domain provided by Railway
2. Login with your configured email and password
3. Add a new server connection using the PostgreSQL service's private domain

## Managing Environment Variables

To add or modify variables after deployment:

1. Go to your project in the Railway dashboard
2. Navigate to the "Variables" tab
3. Click "+ New Variable" to add a new variable
4. Make your changes and save
5. The service will restart automatically with the new variables

## Official Documentation

For a complete guide on using your deployed Letta server, see:
- [Official Letta Railway Deployment Guide](https://docs.letta.com/guides/server/railway)
- [Letta GitHub Repository](https://github.com/letta-ai/letta)

## Troubleshooting

If you encounter issues with your deployment:

1. **Check service logs** in the Railway dashboard
2. **Verify environment variables** are set correctly
3. **Confirm private networking** is properly configured
4. **Restart the service** if necessary

For specific issues:
- **Connection problems**: Verify the domain and ports are correct
- **API failures**: Check that your LLM API keys are valid
- **Database issues**: Use PgAdmin to inspect the database state 