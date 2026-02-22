# Multi-Tenant Task Management SaaS

A production-style multi-tenant task management SaaS built to demonstrate secure authentication, tenant isolation, and scalable cloud architecture.

The project emphasizes backend architecture, clean schema management, and production-aligned deployment to AWS.

---

# 🚀 Architecture Overview

Frontend (React + TypeScript, Vite)
        ↓
Nginx (Static hosting on EC2)
        ↓
ASP.NET Core Web API (Dockerized)
        ↓
PostgreSQL
    - Local: Docker
    - Production: Amazon RDS

---

# 🏢 Multi-Tenancy Design

## Strategy: Shared Database, Shared Schema

- Single PostgreSQL database
- Shared tables
- `TenantId` column on all tenant-scoped entities
- Isolation enforced via EF Core Global Query Filter
- Tenant context derived from signed JWT claim

### Why This Approach?

- Prevents accidental data leakage
- Ensures tenant isolation at ORM level
- Scales efficiently
- Common SaaS industry pattern

---

# 🔐 Authentication & Security

## Authentication

- JWT-based authentication
- TenantId stored in signed JWT claim
- Stateless API design

## Password Storage

- Salted & iterated hashing
- Implemented using ASP.NET Core PasswordHasher
- Protection against brute-force & rainbow table attacks

## Security Principles

- No tenant context accepted from HTTP headers
- All tenant isolation enforced server-side
- HTTPS required in production
- Environment-based configuration
- Sensitive values via environment variables

---

# 📋 Core Features

## Authentication

- Register (creates tenant + admin user)
- Login
- Current user retrieval

## Task Management

- Create task
- Update task
- Soft delete
- Paginated list
- Status filtering

## Tenant Management

- Tenant info retrieval
- User management within tenant

---

# 🗄 Database Strategy

## Local Development

PostgreSQL runs locally via Docker.

### Why Dockerized PostgreSQL?

- Fast schema iteration
- Safe database reset
- Migration testing
- Index tuning
- Concurrency testing
- Same DB engine as production

Start database:

docker-compose up -d postgres

Development connection string:

Host=localhost;
Port=5432;
Database=taskdb;
Username=postgres;
Password=postgres;

---

## Schema Management

Code First approach using Entity Framework Core.

Create migration:

dotnet ef migrations add InitialCreate

Apply migration:

dotnet ef database update

Migrations are tracked in version control.

---

# 📊 Index Strategy

To support multi-tenant scalability:

- Index on TenantId
- Composite index on (TenantId, Status)
- Index on DueDate
- Optional index on CreatedAt

Purpose:

- Fast tenant-scoped queries
- Efficient pagination
- Query performance at scale

---

# ☁ Cloud Deployment Strategy

## Production Environment

Hosted on Amazon Web Services.

Infrastructure:

- EC2 for application hosting
- Dockerized ASP.NET Core API
- Nginx for serving static frontend
- Amazon RDS for PostgreSQL

## Deployment Flow

1. Build frontend:

npm run build

2. Build backend Docker image:

docker build -t task-api .

3. Push image to EC2

4. Provision RDS PostgreSQL

5. Update Production connection string:

Host=<rds-endpoint>;
Port=5432;
Database=taskdb;
Username=<user>;
Password=<password>;

6. Apply migrations:

dotnet ef database update

---

# ⚙ Environment Configuration

Configuration separated by environment:

- appsettings.Development.json
- appsettings.Production.json

Environment variables override secrets in production.

---

# 📦 Running Locally

## 1. Clone repository

git clone <repository-url>

## 2. Start PostgreSQL

docker-compose up -d postgres

## 3. Run backend

dotnet run

## 4. Run frontend

npm install
npm run dev

---

# 🧠 Design Principles

- Secure by default
- Tenant isolation enforced architecturally
- Stateless API design
- Separation of concerns (frontend / backend)
- Infrastructure-aware development
- Production-aligned local development

---

# 📈 Future Improvements

- Role-based authorization (RBAC)
- Audit logging
- Rate limiting
- ETag / Optimistic concurrency control
- External IdP integration
- Horizontal scaling support
- CI/CD pipeline

---

# 🎯 Engineering Focus

This project emphasizes:

- Backend architecture
- Multi-tenant SaaS patterns
- Authentication & security design
- Cloud-ready deployment
- Clean, maintainable code

---

# 📌 Tech Stack

Backend:
- ASP.NET Core Web API
- Entity Framework Core (Code First)
- PostgreSQL
- Docker

Frontend:
- React
- TypeScript
- Vite
- Context API

Infrastructure:
- Amazon EC2
- Amazon RDS
- Nginx