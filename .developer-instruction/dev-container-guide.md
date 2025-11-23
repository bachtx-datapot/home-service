# Dev Container Development Guide

## Overview

This guide explains how to use dev-containers for developing the Home Service application. Dev-containers provide a consistent, reproducible development environment with all necessary tools pre-configured.

## Prerequisites

- Docker Desktop installed and running
- Visual Studio Code with the "Dev Containers" extension

## Dev Container Options

The repository provides three dev-container configurations:

### 1. Root Dev-Container (Recommended)
**Location**: `.devcontainer/devcontainer.json`

This is the recommended approach for full-stack development. It includes:
- Python 3.10 + uv for backend development
- Node.js 24 + npm/pnpm for frontend development
- Docker-in-Docker for running the PostgreSQL database
- All necessary VS Code extensions

**When to use**: When working on both backend and frontend, or when you need full control over all services.

### 2. Backend Dev-Container
**Location**: `backend/.devcontainer/devcontainer.json`

Specialized container for backend development with:
- PostgreSQL database included via Docker Compose
- Python tooling and extensions
- Backend dependencies pre-installed

**When to use**: When focusing exclusively on backend development.

### 3. Frontend Dev-Container
**Location**: `frontend/.devcontainer/devcontainer.json`

Specialized container for frontend development with:
- Node.js 24 environment
- Frontend tooling and extensions
- Frontend dependencies pre-installed

**When to use**: When focusing exclusively on frontend development.

## Getting Started with Root Dev-Container

### Step 1: Open Dev-Container

1. Open the repository folder in VS Code
2. Press `F1` (or `Ctrl+Shift+P` / `Cmd+Shift+P`)
3. Type and select: **"Dev Containers: Reopen in Container"**
4. Wait for the container to build and initialize

On first run, the setup script will:
- Install `uv` for Python package management
- Install `pnpm` for Node.js package management
- Set up backend dependencies

### Step 2: Start the Database

```bash
docker compose up db -d
```

Verify the database is ready:
```bash
docker compose exec db pg_isready -U postgres
```

### Step 3: Start Backend

Option A - Using the helper script:
```bash
bash scripts/start-backend.sh
```

Option B - Manual start:
```bash
cd backend
source .venv/bin/activate
alembic upgrade head
fastapi dev app/main.py
```

Backend will be available at: http://localhost:8000

### Step 4: Start Frontend

Option A - Using the helper script:
```bash
bash scripts/start-frontend.sh
```

Option B - Manual start:
```bash
cd frontend
npm install  # if not already installed
npm run dev
```

Frontend will be available at: http://localhost:5173

### Step 5: Verify Services

Run the health check script:
```bash
bash scripts/check-services.sh
```

This will show the status of:
- PostgreSQL database (port 5432)
- Backend API (port 8000)
- Frontend dev server (port 5173)

## Daily Workflow

### Starting Work

1. Open VS Code in the dev-container (if not already)
2. Start database: `docker compose up db -d`
3. Start backend in terminal 1: `bash scripts/start-backend.sh`
4. Start frontend in terminal 2: `bash scripts/start-frontend.sh`
5. Start coding! 🚀

### During Development

- **Backend changes**: Auto-reload is enabled, just save your files
- **Frontend changes**: Vite will hot-reload automatically
- **Database changes**: Use Alembic migrations (see backend/README.md)
- **Check service status**: Run `bash scripts/check-services.sh` anytime

### Ending Work

1. Stop backend: `Ctrl+C` in backend terminal
2. Stop frontend: `Ctrl+C` in frontend terminal
3. Stop database: `docker compose down`

## Port Forwarding

The dev-container automatically forwards these ports:

| Port | Service | Auto-Forward Behavior |
|------|---------|----------------------|
| 5432 | PostgreSQL | Silent |
| 8000 | Backend API | Notify |
| 5173 | Frontend | Open browser |
| 8080 | Adminer | Ignore |
| 8090 | Traefik | Ignore |
| 1080 | MailCatcher | Ignore |

You can access these services from your host machine at `localhost:<port>`.

## Troubleshooting

### Container fails to build

**Issue**: Dev-container build fails or times out.

**Solution**:
1. Ensure Docker Desktop is running
2. Restart Docker Desktop
3. Try rebuilding: `F1` → "Dev Containers: Rebuild Container"

### Database connection errors

**Issue**: Backend can't connect to database.

**Solution**:
1. Check database is running: `docker compose ps`
2. Verify database health: `docker compose exec db pg_isready -U postgres`
3. Check `.env` file has correct `POSTGRES_SERVER=db`

### Port already in use

**Issue**: Error message about port 8000, 5173, or 5432 already in use.

**Solution**:
```bash
# Find process using the port
lsof -i :8000  # or :5173, :5432

# Kill the process if needed
kill -9 <PID>
```

### Docker-in-Docker not working

**Issue**: Can't run `docker compose` commands inside container.

**Solution**:
1. Ensure the dev-container config includes the Docker-in-Docker feature
2. Rebuild the container: `F1` → "Dev Containers: Rebuild Container"

## Tips and Best Practices

### Use Multiple Terminals

Open multiple terminal instances in VS Code:
- Terminal 1: Backend development server
- Terminal 2: Frontend development server
- Terminal 3: Ad-hoc commands (migrations, testing, etc.)

### Database Management

Access Adminer at http://localhost:8080 to:
- Browse database tables
- Run SQL queries
- Import/export data

Connection details:
- System: PostgreSQL
- Server: db
- Username: postgres
- Password: (from `.env` file)
- Database: app

### Running Tests

Backend tests:
```bash
cd backend
bash scripts/tests-start.sh
```

Frontend tests:
```bash
cd frontend
npm test
```

### VS Code Integration

The dev-container includes these extensions:
- **Python**: IntelliSense, debugging, linting
- **Pylance**: Advanced Python language support
- **Ruff**: Fast Python linter
- **ESLint**: JavaScript/TypeScript linting
- **Prettier**: Code formatting
- **Docker**: Docker file support

### Environment Variables

The `.env` file at the root contains all configuration. Key variables:
```env
POSTGRES_SERVER=db
POSTGRES_PORT=5432
POSTGRES_USER=postgres
POSTGRES_PASSWORD=<your-password>
POSTGRES_DB=app
```

### Keeping Dependencies Updated

Backend:
```bash
cd backend
uv sync  # Update dependencies
```

Frontend:
```bash
cd frontend
npm update  # Update dependencies
```

## Additional Resources

- [Backend README](../backend/README.md) - Backend-specific documentation
- [Development Guide](../development.md) - General development guide
- [Deployment Guide](../deployment.md) - Production deployment
- [Dev Container Startup Workflow](../.agent/workflows/dev-container-startup.md) - Step-by-step workflow

## Support

If you encounter issues not covered in this guide:
1. Check the main [development.md](../development.md) for general Docker Compose usage
2. Review backend/frontend specific READMEs
3. Check Docker logs: `docker compose logs <service-name>`
