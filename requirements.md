# Multi-tenant Task Management SaaS
## Requirements Definition Document
- Version: 1.0
- Last Updated: 2026-02-22
- Author: Shiori Mita

---

# 1. Background and Objective

## 1.1 Background
This project simulates a multi-tenant SaaS application for team-based task management.

## 1.2 Objective
The objective of this project is to:
- Design a scalable multi-tenant architecture
- Ensure data isolation between tenants
- Consider scalability and performance from early stage
- Deploy the system on AWS (EC2 + RDS)

---

# 2. System Overview

## 2.1 System Architecture
- RESTful Web API built with ASP.NET Core
- PostgreSQL as primary database
- JWT-based authentication
- Dockerized deployment
- Hosted on AWS EC2 with RDS

(Insert architecture diagram here)

# 3 Technology Stack
## 3.1 Backend
- ASP.NET Core Web API
- Entity Framework Core (Code First)
- PostgreSQL
- Docker

## 3.2 Frontend
- React
- TypeScript
- Vite
- Context API for global auth state

## 3.3 Infrastructure
- EC2
- RDS
- Nginx


# 4. User Roles

## 4.1 Tenant Admin
- Manage users within tenant
- Manage tasks
- View audit logs

## 4.2 Member
- Create tasks
- Update assigned tasks
- View tasks within tenant

---

# 5. Functional Requirements

## 5.1 Tenant Management
- Create tenant
- Associate users with tenant
- Ensure tenant-based data isolation

## 5.2 Authentication & Authorization
- JWT token issuance
- Role-based authorization
- Secure password hashing

## 5.3 Task Management
- Create task
- Update task
- Delete task
- Change task status
- List tasks with pagination

## 5.4 Audit Logging
- Record task creation/update/delete events
- Store timestamp and user information

---

# 6. Non-Functional Requirements

## 6.1 Scalability
- Initial implementation uses column-based tenant isolation
- Architecture allows migration to schema-level or database-level isolation
- Designed to support horizontal scaling in the future

## 6.2 Performance
- Keyset pagination for large datasets
- Composite index on (TenantId, CreatedAt)
- Avoid N+1 query problems

## 6.3 Availability & Reliability
- Transaction management for data consistency
- Exception handling strategy
- Logging for failure analysis

## 6.4 Security
- JWT authentication
- Password hashing with salt & stretching
- Restricted database access (RDS private access)
- AWS security group configuration
- HTTPS in production
- No tenant ID accepted from client headers

## 6.5 Deployment & Operations
- Dockerized application
- Environment variables for configuration
- EC2 deployment
- RDS managed PostgreSQL

---

# 7. Data Design

## 7.1 Entities
- Tenants
- Users
- Tasks
- AuditLogs

## 7.2 Relationships
- One Tenant has many Users
- One Tenant has many Tasks
- Tasks belong to one Tenant
- AuditLogs track operations per Tenant

(Insert ER diagram here)

## 7.3 Index Strategy
- Composite index (TenantId, CreatedAt)
- Index on UserId where appropriate

---

# 8. API Design Principles

- RESTful design
- Proper HTTP status codes
- Standardized error response format
- Versioning strategy (e.g., /api/v1/)

---

# 9. Concurrency Control Strategy

- Use optimistic concurrency control (RowVersion)
- Prevent lost updates
- Ensure transaction boundary consistency

---

# 10. Deployment Architecture

## 10.1 Infrastructure
- EC2 instance for API hosting
- RDS PostgreSQL for data storage

## 10.2 Security
- RDS not publicly accessible
- SSH restricted by IP
- Environment-based configuration

## 10.3 Future Improvements
- Load balancer (ALB)
- Auto Scaling
- Redis caching layer
- Background job processing

---

# 11. Constraints

- Development period: 1 month
- Redis not implemented (future extension)
- Message queue not implemented (future extension)