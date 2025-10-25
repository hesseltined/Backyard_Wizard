# Backyard Wizard - Setup Guide

**Version:** 1.0.0  
**Date:** 2025-10-25  
**Author:** Doug Hesseltine  
**Copyright:** Technologist.services 2025

---

## Quick Start

This guide walks through setting up the Backyard Wizard development environment.

---

## Prerequisites

### Required Software
- **Node.js:** 18.x or higher
- **Python:** 3.11 or higher
- **Docker:** 24.x or higher
- **Docker Compose:** 2.x or higher
- **Git:** 2.x or higher

### Optional Tools
- **Make:** For convenience commands
- **direnv:** For environment variable management
- **VS Code:** Recommended IDE with extensions

---

## Project Structure

```
Backyard_Wizard/
|
+-- backend/                  # Python FastAPI backend
|   +-- app/
|   |   +-- api/             # API endpoints
|   |   +-- core/            # Config, security, database
|   |   +-- models/          # SQLAlchemy models
|   |   +-- schemas/         # Pydantic schemas
|   |   +-- services/        # Business logic
|   |   +-- workers/         # Celery tasks
|   |   +-- main.py          # FastAPI app entry
|   +-- migrations/          # Alembic migrations
|   +-- tests/
|   +-- requirements.txt
|   +-- Dockerfile
|
+-- frontend/                 # React TypeScript frontend
|   +-- src/
|   |   +-- components/      # React components
|   |   +-- features/        # Feature modules
|   |   +-- hooks/           # Custom hooks
|   |   +-- lib/             # Utilities
|   |   +-- pages/           # Page components
|   |   +-- stores/          # State management
|   |   +-- App.tsx
|   |   +-- main.tsx
|   +-- public/
|   +-- package.json
|   +-- vite.config.ts
|   +-- Dockerfile
|
+-- ml-service/              # ML inference microservice
|   +-- app/
|   |   +-- models/          # ML model definitions
|   |   +-- services/        # Inference logic
|   |   +-- utils/           # Image/video processing
|   |   +-- main.py
|   +-- weights/             # Model weights (gitignored)
|   +-- requirements.txt
|   +-- Dockerfile
|
+-- shared/                  # Shared types, schemas
|   +-- types/
|   +-- schemas/
|
+-- docs/                    # Documentation
|   +-- api/                 # API documentation
|   +-- architecture/        # Architecture decision records
|   +-- guides/              # User and developer guides
|
+-- scripts/                 # Utility scripts
|   +-- seed_data.py         # Database seeding
|   +-- fetch_usda_plants.py # Download USDA PLANTS
|   +-- setup_dev.sh         # Development setup
|
+-- docker/                  # Docker configurations
|   +-- postgres/
|   +-- redis/
|   +-- nginx/
|
+-- .github/                 # GitHub Actions workflows
|   +-- workflows/
|       +-- ci.yml
|       +-- deploy.yml
|
+-- docker-compose.yml       # Local development stack
+-- docker-compose.prod.yml  # Production stack
+-- Makefile                 # Common commands
+-- .env.example             # Environment template
+-- .gitignore
+-- README.md
+-- PROJECT_PLAN.md
+-- SETUP.md
```

---

## Initial Setup

### 1. Clone Repository

```bash
git clone https://github.com/hesseltined/Backyard_Wizard.git
cd Backyard_Wizard
```

### 2. Environment Configuration

```bash
# Copy environment template
cp .env.example .env

# Edit .env with your settings
# Key variables:
#   - POSTGRES_PASSWORD
#   - JWT_SECRET
#   - MAPBOX_API_KEY
#   - AWS_ACCESS_KEY_ID (for S3)
#   - AWS_SECRET_ACCESS_KEY
```

### 3. Start Infrastructure

```bash
# Start Postgres, PostGIS, Redis
docker-compose up -d postgres redis

# Wait for services to be ready
docker-compose ps
```

### 4. Backend Setup

```bash
cd backend

# Create virtual environment
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Run migrations
alembic upgrade head

# Seed initial data
python scripts/seed_data.py

# Start backend
uvicorn app.main:app --reload --port 8000
```

Backend API will be available at: `http://localhost:8000`
API docs: `http://localhost:8000/docs`

### 5. Frontend Setup

```bash
cd frontend

# Install dependencies
npm install

# Start development server
npm run dev
```

Frontend will be available at: `http://localhost:5173`

### 6. ML Service Setup (Optional for Phase 0)

```bash
cd ml-service

# Create virtual environment
python3 -m venv venv
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Download initial model weights
python scripts/download_models.py

# Start ML service
uvicorn app.main:app --port 8001
```

ML API will be available at: `http://localhost:8001`

---

## Docker Compose (All Services)

Alternatively, run all services with Docker Compose:

```bash
# Build all services
docker-compose build

# Start all services
docker-compose up

# Start in background
docker-compose up -d

# View logs
docker-compose logs -f

# Stop all services
docker-compose down

# Reset everything (WARNING: deletes data)
docker-compose down -v
```

---

## Database Setup

### Create Initial Migration

```bash
cd backend
source venv/bin/activate

# Auto-generate migration from models
alembic revision --autogenerate -m "Initial schema"

# Review the generated migration in migrations/versions/

# Apply migration
alembic upgrade head
```

### Seed Development Data

```bash
# Run seed script
python scripts/seed_data.py

# This creates:
#   - Test user account
#   - Sample property
#   - Initial plant catalog (50 species)
#   - Base rules (20 lawn and tree/shrub tasks)
```

---

## Development Workflow

### Running Tests

```bash
# Backend tests
cd backend
pytest

# Frontend tests
cd frontend
npm test

# E2E tests
npm run test:e2e
```

### Code Quality

```bash
# Backend
cd backend
black .           # Format
ruff check .      # Lint
mypy .            # Type check

# Frontend
cd frontend
npm run lint      # ESLint
npm run format    # Prettier
npm run typecheck # TypeScript
```

### Database Migrations

```bash
# Create migration
alembic revision -m "Add new_table"

# Apply migrations
alembic upgrade head

# Rollback last migration
alembic downgrade -1

# View migration history
alembic history
```

---

## Configuration

### Environment Variables

Key environment variables (see `.env.example` for full list):

#### Database
- `DATABASE_URL`: PostgreSQL connection string
- `POSTGRES_DB`: Database name
- `POSTGRES_USER`: Database user
- `POSTGRES_PASSWORD`: Database password

#### Authentication
- `JWT_SECRET`: Secret for JWT signing
- `JWT_ALGORITHM`: Algorithm (HS256)
- `ACCESS_TOKEN_EXPIRE_MINUTES`: Token expiry

#### External APIs
- `MAPBOX_API_KEY`: Mapbox API key (free tier)
- `OPEN_METEO_BASE_URL`: Open-Meteo API URL
- `USANPN_API_KEY`: USA-NPN API key (optional)

#### Storage
- `AWS_ACCESS_KEY_ID`: S3 access key
- `AWS_SECRET_ACCESS_KEY`: S3 secret
- `AWS_S3_BUCKET`: Bucket name
- `AWS_REGION`: AWS region

#### Redis
- `REDIS_URL`: Redis connection string
- `CELERY_BROKER_URL`: Celery broker URL

---

## Common Commands

### Using Make

```bash
# Setup development environment
make setup

# Start all services
make up

# Run tests
make test

# Lint and format
make lint

# Clean up
make clean

# View logs
make logs

# Database migrations
make migrate

# Seed data
make seed
```

---

## Troubleshooting

### Port Conflicts
If ports 5173, 8000, 5432, or 6379 are in use:
```bash
# Check what's using a port
lsof -i :5173

# Kill process
kill -9 <PID>

# Or change ports in docker-compose.yml and .env
```

### Database Connection Issues
```bash
# Check if Postgres is running
docker-compose ps postgres

# View Postgres logs
docker-compose logs postgres

# Connect to database
docker-compose exec postgres psql -U backyard_wizard -d backyard_wizard_db
```

### Frontend Build Issues
```bash
# Clear node_modules and reinstall
cd frontend
rm -rf node_modules package-lock.json
npm install

# Clear Vite cache
rm -rf node_modules/.vite
```

### Backend Import Errors
```bash
# Ensure virtual environment is activated
source venv/bin/activate

# Reinstall dependencies
pip install -r requirements.txt

# Check Python path
python -c "import sys; print(sys.path)"
```

---

## Next Steps

After completing setup:

1. Review `PROJECT_PLAN.md` for development roadmap
2. Check `docs/architecture/` for system design
3. Explore API docs at `http://localhost:8000/docs`
4. Run test suite to verify setup
5. Start with Week 1 tasks from Phase 0

---

## Getting Help

- **Documentation:** Check `docs/` directory
- **Issues:** Report bugs on GitHub Issues
- **Discussions:** Use GitHub Discussions for questions

---

## Resources

### Official Documentation
- [FastAPI](https://fastapi.tiangolo.com/)
- [React](https://react.dev/)
- [Mapbox GL JS](https://docs.mapbox.com/mapbox-gl-js/)
- [PostgreSQL](https://www.postgresql.org/docs/)
- [PostGIS](https://postgis.net/documentation/)

### Data Sources
- [Open-Meteo API](https://open-meteo.com/)
- [USDA PLANTS](https://plants.usda.gov/)
- [USA-NPN](https://www.usanpn.org/data)
- [NOAA NWS API](https://www.weather.gov/documentation/services-web-api)

---

**End of Setup Guide**
