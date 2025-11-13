# Project Initiation Standard for Cursor IDE
## Enterprise-Grade Development Framework for Sub-0.2% Error Rate

**Document Version:** 1.0  
**Last Updated:** November 10, 2025  
**Purpose:** Establish rigorous standards for project initiation to achieve world-class code quality

---

## Executive Summary

This document defines the mandatory framework for initiating all software development projects. Following this standard reduces production errors to below 0.2% through systematic planning, comprehensive documentation, and rigorous quality gates.

**Key Principles:**
- **Plan Before Code:** 80% planning, 20% execution
- **Document Everything:** If it's not documented, it doesn't exist
- **Validate Early:** Catch errors in specification, not production
- **Standardize Always:** Consistency eliminates cognitive load
- **Automate Ruthlessly:** Humans should not do what machines can do

---

## Table of Contents

1. [Pre-Project Checklist](#1-pre-project-checklist)
2. [The 10-Step Methodology](#2-the-10-step-methodology)
3. [Project Specification Document](#3-project-specification-document)
4. [Documentation Package Requirements](#4-documentation-package-requirements)
5. [Cursor IDE Configuration](#5-cursor-ide-configuration)
6. [Code Quality Standards](#6-code-quality-standards)
7. [Error Prevention Framework](#7-error-prevention-framework)
8. [Quality Gates & Checkpoints](#8-quality-gates--checkpoints)
9. [AI Agent Utilization](#9-ai-agent-utilization)
10. [Templates & Checklists](#10-templates--checklists)

---

## 1. Pre-Project Checklist

Before writing a single line of code, complete this mandatory checklist:

### 1.1 Business Validation
- [ ] **Business case approved** by project sponsor and stakeholders
- [ ] **ROI calculation** completed with 3-year projection
- [ ] **Stakeholder map** created with roles and approval authority
- [ ] **Success metrics** defined (KPIs, OKRs, and acceptance criteria)
- [ ] **Risk assessment** completed with mitigation strategies
- [ ] **Budget allocation** confirmed with appropriate approval

### 1.2 Technical Validation
- [ ] **Technology stack** approved by architecture team
- [ ] **Infrastructure requirements** assessed and costed
- [ ] **Security review** completed for data classification
- [ ] **Compliance check** performed (VAT, GAAP, IFRS, local regulations)
- [ ] **Integration points** mapped with existing systems
- [ ] **Performance requirements** quantified (response times, throughput, concurrency)

### 1.3 Resource Validation
- [ ] **Team composition** finalized with skill matrix
- [ ] **Development timeline** approved with milestone dates
- [ ] **Third-party dependencies** identified and assessed
- [ ] **Training requirements** identified for team upskilling
- [ ] **Support model** defined for post-deployment
- [ ] **Disaster recovery plan** outlined

### 1.4 Documentation Validation
- [ ] **Project specification document** template prepared
- [ ] **Documentation repository** created and access granted
- [ ] **Version control strategy** defined (branching model, PR process)
- [ ] **Change management process** established
- [ ] **Communication plan** created (status reports, demos, reviews)

**🚨 GATE 0:** All checklist items must be completed before proceeding to Step 1

---

## 2. The 10-Step Methodology

This methodology ensures systematic project development with built-in quality controls.

### Step 1: Business Context
**Purpose:** Establish the "why" behind the project

**Required Deliverables:**
- Business problem statement (max 3 paragraphs)
- Current state analysis with pain points
- Desired future state description
- Expected business outcomes (quantified)
- Organizational impact assessment
- Regulatory/compliance requirements

**Example Template:**
```
BUSINESS PROBLEM:
The [process name] currently takes [X time] with [Y%] error rate due to 
[specific pain point].

CURRENT STATE:
- Manual/semi-automated process with [specific tools]
- [Number] of integration points or data sources
- Limited visibility into [key metrics]
- Errors discovered [when/how]

DESIRED STATE:
- Automated process completing in [target time]
- Error rate below 0.2%
- Real-time visibility with [specific capabilities]
- Automated validation and error detection

BUSINESS OUTCOMES:
- Reduce team workload by [X hours/month] ($[Y] annual savings)
- Enable faster [decision-making/processing/other benefit]
- Improve [quality metric] for [stakeholders]
- Support [scalability goal] without additional resources
```

**Validation Criteria:**
- [ ] Executive sponsor has approved problem statement
- [ ] Current state metrics are quantified and verified
- [ ] Future state is achievable within project constraints
- [ ] Business outcomes are SMART (Specific, Measurable, Achievable, Relevant, Time-bound)

---

### Step 2: Requirements
**Purpose:** Define what the system must do (functional) and how well it must do it (non-functional)

**Required Deliverables:**

#### 2.1 Functional Requirements
Structure: User story format with acceptance criteria
```
As a [role], I want to [action], so that [benefit]

ACCEPTANCE CRITERIA:
- Given [context]
- When [action]
- Then [expected result]
```

**Categories to cover:**
- User management & authentication
- Core business logic
- Data input & validation
- Reporting & analytics
- Integration with existing systems
- Workflow & approval processes

#### 2.2 Non-Functional Requirements
Must specify exact metrics for:

**Performance:**
- Page load time: < X seconds (95th percentile)
- API response time: < Y milliseconds
- Concurrent users supported: Z users
- Database query performance: < N ms for 95% of queries

**Scalability:**
- Data volume projection: X records/year for Y years
- User growth projection: Current → Target users
- Transaction volume: X transactions/day

**Security:**
- Authentication method (SSO, MFA requirements)
- Authorization model (RBAC, ABAC)
- Data encryption (at rest, in transit)
- Audit logging requirements
- PII/sensitive data handling

**Availability:**
- Uptime SLA: X% (e.g., 99.9% = 43 minutes downtime/month)
- Maintenance windows: When and how often
- Backup frequency and retention
- Recovery time objective (RTO)
- Recovery point objective (RPO)

**Compliance:**
- Industry-specific reporting requirements
- Regulatory compliance (financial, healthcare, data protection, etc.)
- Data residency and sovereignty requirements
- Audit trail and logging requirements
- Document retention and archival policies

**Usability:**
- Supported browsers and versions
- Mobile responsiveness requirements
- Accessibility standards (WCAG 2.1 Level AA)
- Multi-language support (if applicable)
- Training and documentation requirements

**Example:**
```
NFR-PERF-001: Dashboard Load Time
- Requirement: Main dashboard must load in ≤2 seconds for 95% of requests
- Measurement: Google Lighthouse performance score ≥90
- Test condition: 50 concurrent users, 1,000,000 records in database
- Acceptance: Load tested and documented before production deployment

NFR-SEC-002: Sensitive Data Security
- Requirement: All sensitive data must be encrypted at rest and in transit
- Implementation: AES-256 encryption (at rest), TLS 1.3 (in transit)
- Access control: Role-based access with principle of least privilege
- Audit: All access logged with user, timestamp, and action
```

**Validation Criteria:**
- [ ] All functional requirements have acceptance criteria
- [ ] Non-functional requirements are quantified with specific metrics
- [ ] Requirements are testable and measurable
- [ ] Requirements are prioritized (Must-have, Should-have, Nice-to-have)
- [ ] Requirements traceability matrix created
- [ ] Business stakeholders have signed off

---

### Step 3: Constraints
**Purpose:** Document the boundaries within which the solution must operate

**Required Deliverables:**

#### 3.1 Technical Constraints
- **Technology mandates:** Required languages, frameworks, or platforms
- **Legacy ```
Entity: User
Purpose: Stores application user accounts and authentication data
Access Pattern: Read-heavy (authentication on every request)

Fields:
- id: UUID, Primary Key, Non-null, Indexed
- email: VARCHAR(255), Unique, Non-null, Indexed, Lowercase
- password_hash: VARCHAR(255), Non-null (hashed with bcrypt, cost=12)
- display_name: VARCHAR(100), Non-null
- role: ENUM('admin', 'manager', 'user'), Non-null, Default: 'user', Indexed
- email_verified: BOOLEAN, Non-null, Default: false
- last_login_at: TIMESTAMP, Nullable
- created_at: TIMESTAMP, Non-null, Default: now(), Indexed
- updated_at: TIMESTAMP, Non-null, Auto-update
- deleted_at: TIMESTAMP, Nullable (soft delete)

Relationships:
- One-to-many with AuditLog on user_id
- Many-to-many with Organization through UserOrganization

Business Rules:
- Email must be unique across non-deleted records
- Password must meet complexity requirements (min 12 chars, upper, lower, number, special)
- Soft delete only (never hard delete for audit trail)
- Last login updated on every successful authentication

Indexes:
- Primary: id
- Unique: email (WHERE deleted_at IS NULL)
- Secondary: role (for role-based queries)
- Secondary: created_at (for user growth analytics)

Estimated Volume:
- Year 1: 500 users
- Year 3: 2,000 users
- Year 5: 5,000 users
```

**Validation Criteria:**
- [ ] ERD reviewed and approved by technical lead
- [ ] All entities have clear purpose and access patterns
- [ ] Relationships enforce referential integrity
- [ ] Indexes align with query patterns in requirements
- [ ] Data types are appropriate for expected data
- [ ] Schema supports all functional requirements
- [ ] Data volume estimates validated with growth projections

---

### Step 6: API Contract
**Purpose:** Define the interface between frontend and backend (or between services)

**Required Deliverables:**

#### 6.1 API Specification Document
Use OpenAPI 3.0 (Swagger) or GraphQL schema format.

For each endpoint, document:

**REST API Template:**
```
Endpoint: POST /api/v1/users
Purpose: Create a new user account
Authentication: Required (Admin role only)
Rate Limit: 100 requests/hour per IP

Request Headers:
- Authorization: Bearer {jwt_token}
- Content-Type: application/json

Request Body:
{
  "email": "string (required, email format, max 255 chars)",
  "displayName": "string (required, max 100 chars)",
  "role": "string (optional, enum: admin|manager|user, default: user)"
}

Success Response (201 Created):
{
  "id": "uuid",
  "email": "string",
  "displayName": "string",
  "role": "string",
  "emailVerified": false,
  "createdAt": "ISO 8601 timestamp"
}

Error Responses:
- 400 Bad Request: Invalid input data
  {
    "error": "VALIDATION_ERROR",
    "message": "Invalid email format",
    "fields": {
      "email": "Must be a valid email address"
    }
  }
- 401 Unauthorized: Missing or invalid authentication token
- 403 Forbidden: User lacks admin role
- 409 Conflict: Email already exists
- 429 Too Many Requests: Rate limit exceeded
- 500 Internal Server Error: Server-side error

Validation Rules:
- Email: Valid format, lowercase, trim whitespace
- Display Name: Min 2 chars, max 100 chars, trim whitespace
- Role: Must be one of: admin, manager, user

Example Request:
POST /api/v1/users
Authorization: Bearer eyJhbGciOiJIUzI1NiIs...
Content-Type: application/json

{
  "email": "jane.doe@example.com",
  "displayName": "Jane Doe",
  "role": "manager"
}

Example Response:
201 Created

{
  "id": "550e8400-e29b-41d4-a716-446655440000",
  "email": "jane.doe@example.com",
  "displayName": "Jane Doe",
  "role": "manager",
  "emailVerified": false,
  "createdAt": "2025-11-10T14:30:00Z"
}

Performance SLA:
- p50: < 100ms
- p95: < 300ms
- p99: < 500ms
```

#### 6.2 API Conventions
Document project-wide API standards:

**Naming Conventions:**
- Endpoints: Use kebab-case (`/api/users/reset-password`)
- JSON keys: Use camelCase (`firstName`, `createdAt`)
- Query parameters: Use snake_case (`?sort_by=created_at&page_size=20`)

**Versioning:**
- Strategy: URL versioning (`/api/v1/`, `/api/v2/`)
- Deprecation policy: 6-month notice before removal
- Version support: Current + 1 previous version

**Pagination:**
```
Request: GET /api/v1/users?page=2&page_size=20
Response:
{
  "data": [...],
  "pagination": {
    "page": 2,
    "pageSize": 20,
    "totalPages": 10,
    "totalItems": 200,
    "hasNext": true,
    "hasPrevious": true
  }
}
```

**Filtering and Sorting:**
```
GET /api/v1/users?role=admin&sort_by=created_at&order=desc
```

**Error Response Format:**
```
{
  "error": "ERROR_CODE",
  "message": "Human-readable error message",
  "fields": {
    "fieldName": "Field-specific error message"
  },
  "requestId": "uuid (for support troubleshooting)",
  "timestamp": "ISO 8601 timestamp"
}
```

**Authentication:**
- Method: JWT (JSON Web Tokens)
- Header: `Authorization: Bearer {token}`
- Token expiry: 24 hours
- Refresh token: 30 days

**Rate Limiting:**
- Authenticated users: 1000 requests/hour
- Unauthenticated: 100 requests/hour
- Response headers: `X-RateLimit-Limit`, `X-RateLimit-Remaining`, `X-RateLimit-Reset`

#### 6.3 API Testing Strategy
- **Contract testing:** Ensure API matches specification (using Pact, Dredd)
- **Integration testing:** Test API with real database
- **Load testing:** Validate performance under load (using k6, Artillery)
- **Security testing:** Test authentication, authorization, injection attacks

**Validation Criteria:**
- [ ] All endpoints are documented with examples
- [ ] Error responses cover all failure scenarios
- [ ] Authentication and authorization are clearly specified
- [ ] Rate limits are defined and justified
- [ ] API versioning strategy is in place
- [ ] Performance SLAs are specified for each endpoint
- [ ] Frontend and backend teams have reviewed and approved

---

### Step 7: Setup
**Purpose:** Provide clear instructions for setting up the development environment

**Required Deliverables:**

#### 7.1 Prerequisites
List all required software and versions:
```
Required Software:
- Node.js: v20.x or higher (LTS)
- npm/yarn/pnpm: Latest version
- PostgreSQL: 16.x
- Redis: 7.x (if applicable)
- Docker: 24.x and Docker Compose v2.x
- Git: 2.40.x or higher

Optional Software:
- Cursor IDE (recommended)
- Postman or similar API testing tool
- TablePlus or DBeaver for database management
```

#### 7.2 Development Environment Setup

**Step-by-step guide:**
```bash
# 1. Clone the repository
git clone https://github.com/your-org/your-project.git
cd your-project

# 2. Install dependencies
npm install

# 3. Set up environment variables
cp .env.example .env
# Edit .env file with your local configuration

# 4. Start local database (using Docker)
docker-compose up -d postgres redis

# 5. Run database migrations
npm run db:migrate

# 6. Seed database with sample data (optional)
npm run db:seed

# 7. Start development server
npm run dev

# Application should now be running at:
# - Frontend: http://localhost:3000
# - API: http://localhost:3000/api
# - Database: localhost:5432
```

#### 7.3 Environment Variables
Document all environment variables:

```
# .env.example

# Application
NODE_ENV=development
PORT=3000
APP_URL=http://localhost:3000

# Database
DATABASE_URL=postgresql://user:password@localhost:5432/dbname
DATABASE_POOL_MIN=2
DATABASE_POOL_MAX=10

# Authentication
JWT_SECRET=your-secret-key-min-32-chars
JWT_EXPIRY=24h
REFRESH_TOKEN_EXPIRY=30d

# Email (for development, use Mailtrap or similar)
SMTP_HOST=smtp.mailtrap.io
SMTP_PORT=2525
SMTP_USER=your-username
SMTP_PASS=your-password
FROM_EMAIL=noreply@example.com

# External APIs (if applicable)
STRIPE_SECRET_KEY=sk_test_...
STRIPE_PUBLISHABLE_KEY=pk_test_...

# Monitoring (optional in development)
SENTRY_DSN=https://...
LOG_LEVEL=debug
```

**Security Note:** Never commit `.env` file to version control. Always use `.env.example` as template.

#### 7.4 Development Scripts
Document all npm/package.json scripts:

```json
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "eslint . --ext .ts,.tsx --max-warnings 0",
    "lint:fix": "eslint . --ext .ts,.tsx --fix",
    "format": "prettier --write .",
    "format:check": "prettier --check .",
    "type-check": "tsc --noEmit",
    "test": "vitest",
    "test:ui": "vitest --ui",
    "test:coverage": "vitest --coverage",
    "test:e2e": "playwright test",
    "db:migrate": "prisma migrate dev",
    "db:migrate:prod": "prisma migrate deploy",
    "db:seed": "tsx prisma/seed.ts",
    "db:studio": "prisma studio",
    "db:reset": "prisma migrate reset",
    "prepare": "husky install"
  }
}
```

#### 7.5 Troubleshooting Guide
Common setup issues and solutions:

**Issue:** Port 3000 already in use  
**Solution:** `lsof -ti:3000 | xargs kill -9` or change PORT in .env

**Issue:** Database connection refused  
**Solution:** Ensure Docker containers are running (`docker-compose ps`)

**Issue:** Module not found errors  
**Solution:** Delete node_modules and package-lock.json, then `npm install`

**Issue:** TypeScript errors after pulling latest  
**Solution:** `npm run type-check` to see errors, may need to update types

**Validation Criteria:**
- [ ] Setup instructions tested on clean machine
- [ ] All team members can successfully set up environment
- [ ] Setup time is under 30 minutes
- [ ] All environment variables are documented
- [ ] Troubleshooting guide covers common issues

---

### Step 8: Documentation
**Purpose:** Create comprehensive documentation for development, deployment, and operations

**Required Deliverables:**

#### 8.1 README.md
Project overview and quick start guide:

```markdown
# Project Name

Brief description of what the project does and its main purpose.

## Features
- Feature 1
- Feature 2
- Feature 3

## Tech Stack
- Frontend: Next.js 14, React 18, TypeScript
- Backend: Next.js API Routes, Prisma
- Database: PostgreSQL 16
- Hosting: Vercel

## Quick Start
[Link to setup instructions]

## Documentation
- [Architecture Documentation](docs/ARCHITECTURE.md)
- [API Documentation](docs/API.md)
- [Deployment Guide](docs/DEPLOYMENT.md)
- [Contributing Guidelines](docs/CONTRIBUTING.md)

## License
[License information]
```

#### 8.2 ARCHITECTURE.md
System architecture and design decisions:

**Contents:**
- High-level architecture diagram
- Technology stack justification
- Data flow diagrams
- Security architecture
- Scalability considerations
- Design patterns used
- Key architectural decisions and trade-offs

#### 8.3 API.md
Complete API reference:

**Contents:**
- Authentication guide
- All endpoints with request/response examples
- Error codes and handling
- Rate limiting information
- Webhook documentation (if applicable)
- API versioning policy

#### 8.4 DEPLOYMENT.md
Deployment procedures and infrastructure:

**Contents:**
- Infrastructure diagram
- Deployment pipeline (CI/CD)
- Environment configuration (dev, staging, production)
- Database migration procedures
- Rollback procedures
- Monitoring and alerting setup
- Backup and disaster recovery

#### 8.5 TESTING.md
Testing strategy and guidelines:

**Contents:**
- Testing pyramid (unit, integration, E2E percentages)
- How to run tests
- Writing new tests guidelines
- Code coverage requirements (e.g., 80% minimum)
- Performance testing approach
- Security testing checklist

#### 8.6 CONTRIBUTING.md
Development workflow and standards:

**Contents:**
- Git workflow (branch naming, commit messages)
- Code review process
- Pull request template
- Coding standards and style guide
- Definition of Done checklist

#### 8.7 RUNBOOK.md
Operational procedures:

**Contents:**
- How to investigate common issues
- Log locations and analysis
- Performance tuning
- Database maintenance procedures
- Security incident response
- Escalation procedures

**Validation Criteria:**
- [ ] All documentation is current and accurate
- [ ] Documentation is in markdown format for version control
- [ ] Code examples in docs are tested and working
- [ ] Documentation is organized and easy to navigate
- [ ] Diagrams are included where helpful
- [ ] Documentation covers all aspects of the system

---

### Step 9: MVP Definition
**Purpose:** Define the Minimum Viable Product scope for first release

**Required Deliverables:**

#### 9.1 MVP Scope
**What's In:**
List features that MUST be in the first release:
- Feature 1: [Description and justification]
- Feature 2: [Description and justification]
- Feature 3: [Description and justification]

**What's Out:**
List features that are deferred to future releases:
- Feature X: [Why it's not in MVP, planned for Phase 2]
- Feature Y: [Why it's not in MVP, planned for Phase 3]

**MVP Success Criteria:**
- [ ] User can successfully [core action 1]
- [ ] User can successfully [core action 2]
- [ ] System meets [specific performance metric]
- [ ] All critical security requirements implemented
- [ ] Basic reporting/analytics available

#### 9.2 MVP Timeline
```
Week 1-2: Setup & Architecture
- Environment setup
- Database schema implementation
- API scaffolding
- Authentication system

Week 3-4: Core Features (Backend)
- Feature 1 API endpoints
- Feature 2 API endpoints
- Business logic implementation
- Unit tests for critical paths

Week 5-6: Core Features (Frontend)
- Feature 1 UI
- Feature 2 UI
- Form validation
- Error handling

Week 7-8: Integration & Testing
- End-to-end testing
- Bug fixes
- Performance optimization
- Security audit

Week 9: Deployment Prep
- Production environment setup
- Data migration (if applicable)
- Load testing
- Documentation finalization

Week 10: MVP Launch
- Production deployment
- Monitoring setup
- User training
- Support readiness
```

#### 9.3 MVP User Stories
Prioritized list of user stories for MVP:

```
Priority: CRITICAL (Must-have for launch)
- As a user, I can register an account so that I can access the system
- As a user, I can log in securely so that I can access my data
- As a user, I can [core action] so that I can [achieve key outcome]

Priority: HIGH (Important for MVP)
- As a user, I can reset my password so that I can regain access if forgotten
- As an admin, I can manage users so that I can control access
- As a user, I can view [key information] so that I can make decisions

Priority: MEDIUM (Nice to have in MVP)
- As a user, I can export data to CSV so that I can analyze in Excel
- As a user, I can receive email notifications so that I stay informed
```

#### 9.4 Post-MVP Roadmap
**Phase 2 (Months 2-4):**
- Advanced reporting and analytics
- Bulk operations
- Mobile responsive improvements
- Integration with System X

**Phase 3 (Months 5-6):**
- Advanced user permissions
- Workflow automation
- API for third-party integrations
- Multi-language support

**Validation Criteria:**
- [ ] MVP scope is achievable within timeline
- [ ] MVP delivers meaningful value to users
- [ ] All must-have features are clearly defined
- [ ] Post-MVP roadmap is prioritized
- [ ] Stakeholders agree on MVP scope

---

### Step 10: Build Sequence
**Purpose:** Define the exact order of implementation to minimize rework

**Required Deliverables:**

#### 10.1 Implementation Phases
Break down the build into sequential phases:

**Phase 1: Foundation (Week 1-2)**
```
1.1 Project Setup
- [ ] Initialize repository with .gitignore, README
- [ ] Set up Next.js project with TypeScript
- [ ] Configure ESLint, Prettier, Husky
- [ ] Set up Docker Compose for local services
- [ ] Create environment variable templates

1.2 Database Layer
- [ ] Design and review database schema
- [ ] Set up Prisma with PostgreSQL
- [ ] Create initial migration
- [ ] Add seed data for development
- [ ] Test database connection and migrations

1.3 Authentication Foundation
- [ ] Implement user model and migrations
- [ ] Set up JWT authentication
- [ ] Create login/register API endpoints
- [ ] Add password hashing (bcrypt)
- [ ] Implement JWT middleware
- [ ] Add refresh token logic
```

**Phase 2: Core Backend (Week 3-4)**
```
2.1 Business Logic Layer
- [ ] Implement core domain models
- [ ] Create service layer for business logic
- [ ] Add validation with Zod
- [ ] Implement error handling middleware
- [ ] Add logging infrastructure

2.2 API Endpoints
- [ ] Create REST API structure
- [ ] Implement CRUD endpoints for Entity A
- [ ] Implement CRUD endpoints for Entity B
- [ ] Add pagination, filtering, sorting
- [ ] Implement rate limiting
- [ ] Add API documentation (Swagger)

2.3 Testing
- [ ] Write unit tests for services
- [ ] Write integration tests for APIs
- [ ] Set up test database
- [ ] Achieve 80% code coverage
- [ ] Set up CI pipeline for automated testing
```

**Phase 3: Frontend Foundation (Week 5-6)**
```
3.1 UI Setup
- [ ] Set up component library (shadcn/ui)
- [ ] Create layout components (Header, Sidebar, Footer)
- [ ] Implement routing structure
- [ ] Set up Tailwind CSS configuration
- [ ] Create design system tokens (colors, spacing, typography)

3.2 Authentication UI
- [ ] Create login page
- [ ] Create registration page
- [ ] Create password reset flow
- [ ] Implement auth state management
- [ ] Add protected route logic
- [ ] Handle token refresh

3.3 Core Features UI
- [ ] Create dashboard/home page
- [ ] Implement Feature A interface
- [ ] Implement Feature B interface
- [ ] Add form validation
- [ ] Implement error states
- [ ] Add loading states
```

**Phase 4: Integration & Polish (Week 7-8)**
```
4.1 End-to-End Integration
- [ ] Connect frontend to backend APIs
- [ ] Test complete user flows
- [ ] Fix integration bugs
- [ ] Optimize API calls (reduce redundancy)
- [ ] Implement caching where beneficial

4.2 User Experience
- [ ] Add toast notifications
- [ ] Implement confirmation dialogs
- [ ] Add keyboard shortcuts
- [ ] Improve error messages (user-friendly)
- [ ] Add empty states
- [ ] Implement search functionality

4.3 Performance Optimization
- [ ] Code splitting and lazy loading
- [ ] Image optimization
- [ ] Database query optimization
- [ ] Add indexes for slow queries
- [ ] Implement API response caching
- [ ] Run Lighthouse audit (target: 90+)
```

**Phase 5: Security & Quality (Week 9)**
```
5.1 Security Hardening
- [ ] Security audit checklist completed
- [ ] SQL injection testing
- [ ] XSS prevention verification
- [ ] CSRF protection implemented
- [ ] Rate limiting tested
- [ ] Input validation audit
- [ ] Dependency vulnerability scan

5.2 Testing & Quality
- [ ] E2E tests with Playwright
- [ ] Load testing (target: 100 concurrent users)
- [ ] Stress testing to find breaking point
- [ ] Security penetration testing
- [ ] Accessibility audit (WCAG 2.1 AA)
- [ ] Cross-browser testing

5.3 Documentation
- [ ] API documentation complete
- [ ] User documentation/help section
- [ ] Admin documentation
- [ ] Deployment runbook
- [ ] Troubleshooting guide
```

**Phase 6: Deployment (Week 10)**
```
6.1 Production Preparation
- [ ] Set up production environment
- [ ] Configure production database
- [ ] Set up monitoring (Sentry, DataDog, etc.)
- [ ] Configure backup strategy
- [ ] Set up SSL certificates
- [ ] Configure CDN

6.2 Deployment
- [ ] Deploy to staging environment
- [ ] Run smoke tests on staging
- [ ] Stakeholder UAT on staging
- [ ] Deploy to production
- [ ] Run smoke tests on production
- [ ] Monitor for errors (first 24 hours)

6.3 Post-Launch
- [ ] User training sessions
- [ ] Create support documentation
- [ ] Set up support ticketing
- [ ] Collect initial user feedback
- [ ] Plan first iteration improvements
```

#### 10.2 Daily Progress Tracking
Template for tracking daily progress:

```
Date: 2025-11-10
Developer: [Name]
Phase: 2.1 Business Logic Layer

Completed Today:
- [ ] Implemented User service with CRUD operations
- [ ] Added Zod validation schemas for User endpoints
- [ ] Wrote unit tests for User service (15 tests, all passing)

Blocked/Issues:
- None

Tomorrow's Plan:
- [ ] Implement Organization service
- [ ] Add organization-user relationship logic
- [ ] Write tests for Organization service

Notes:
- Discovered edge case with email validation, added additional test
- Code review scheduled for tomorrow 2pm
```

#### 10.3 Build Sequence Principles

**1. Vertical Slices Over Horizontal Layers**
- Build one complete feature (database → API → UI) before moving to next
- This allows for early user testing and feedback
- Reduces integration risk

**2. Test As You Build**
- Write tests alongside implementation
- Never defer testing to "later"
- Aim for 80% code coverage minimum

**3. Demo-Driven Development**
- Schedule demo at end of each phase
- Forces completion of working features
- Gets early stakeholder feedback

**4. Dependency-First Approach**
- Build foundational pieces before dependent features
- Example: Authentication before protected features
- Reduces rework and blocks

**5. Risk-First Implementation**
- Tackle technically risky items early
- Example: Complex calculations, third-party integrations
- Allows time for problem-solving

**Validation Criteria:**
- [ ] Build sequence is logical and minimizes dependencies
- [ ] Each phase has clear deliverables
- [ ] Testing is integrated throughout, not saved for end
- [ ] High-risk items are scheduled early
- [ ] Phases align with team capacity and timeline

---

## 3. Project Specification Document

Before starting any project in Cursor, create a comprehensive specification document using this template:

```markdown
# [Project Name] - Technical Specification

## Project Metadata
- **Project Name:** [Name]
- **Version:** 1.0
- **Date:** [Date]
- **Author:** [Your Name]
- **Status:** Draft | In Review | Approved

---

## 1. Business Context
[Copy from Step 1 methodology]

## 2. Requirements
[Copy from Step 2 methodology]

## 3. Constraints
[Copy from Step 3 methodology]

## 4. Tech Stack
[Copy from Step 4 methodology]

## 5. Data Model
[Copy from Step 5 methodology]

## 6. API Contract
[Copy from Step 6 methodology]

## 7. Setup Instructions
[Copy from Step 7 methodology]

## 8. Documentation Plan
[Copy from Step 8 methodology]

## 9. MVP Definition
[Copy from Step 9 methodology]

## 10. Build Sequence
[Copy from Step 10 methodology]

---

## Appendices

### A. Glossary
Define domain-specific terms

### B. References
Link to external documentation, standards, regulations

### C. Assumptions
List any assumptions made during specification

### D. Risks and Mitigations
Document identified risks and mitigation strategies

### E. Change Log
Track specification changes over time
```

**How to Use with Cursor:**
1. Create this specification document in your project repository
2. Review and refine with team before coding
3. Share with Cursor AI to provide full context
4. Update as project evolves
5. Use as single source of truth for all development decisions

---

## 4. Documentation Package Requirements

Every project MUST include these six core documents:

### 4.1 README.md
**Purpose:** Project overview and quick start  
**Location:** Root directory  
**Length:** 1-2 pages  
**Contents:**
- Project description and purpose
- Key features
- Tech stack summary
- Quick start guide (< 5 steps)
- Links to other documentation
- License and contribution info

### 4.2 ARCHITECTURE.md
**Purpose:** Technical architecture documentation  
**Location:** /docs directory  
**Length:** 5-10 pages  
**Contents:**
- System architecture diagram
- Technology stack details
- Data flow diagrams
- Security architecture
- Design patterns
- Scalability considerations
- Technical decisions log

### 4.3 API.md
**Purpose:** Complete API reference  
**Location:** /docs directory  
**Length:** Varies by API size  
**Contents:**
- Authentication guide
- All endpoints documented
- Request/response examples
- Error codes
- Rate limiting
- Webhook documentation
- Versioning policy

### 4.4 DEPLOYMENT.md
**Purpose:** Deployment and infrastructure guide  
**Location:** /docs directory  
**Length:** 3-5 pages  
**Contents:**
- Infrastructure diagram
- CI/CD pipeline
- Environment setup (dev, staging, prod)
- Deployment procedures
- Rollback procedures
- Monitoring setup
- Backup and DR procedures

### 4.5 TESTING.md
**Purpose:** Testing strategy and guidelines  
**Location:** /docs directory  
**Length:** 2-4 pages  
**Contents:**
- Testing strategy overview
- How to run tests
- Writing new tests
- Code coverage requirements
- Performance testing
- Security testing

### 4.6 RUNBOOK.md
**Purpose:** Operational procedures  
**Location:** /docs directory  
**Length:** 3-5 pages  
**Contents:**
- Common issues and solutions
- Log analysis guide
- Performance tuning
- Database maintenance
- Security incident response
- Escalation procedures

---

## 5. Cursor IDE Configuration

### 5.1 Cursor Rules File
Create a `.cursorrules` file in project root:

```yaml
# Project: [Project Name]
# Purpose: Guide Cursor AI in maintaining code quality and consistency

## Code Quality Standards
- TypeScript strict mode always enabled
- ESLint errors = 0, warnings = 0
- Prettier formatting enforced
- All functions must have JSDoc comments
- All exports must be explicitly typed
- No 'any' types unless absolutely necessary with explanation

## Testing Requirements
- All new features must include unit tests
- Critical paths must have integration tests
- Aim for 80% code coverage minimum
- Tests must pass before committing

## Security Requirements
- No hardcoded secrets or API keys
- All user input must be validated
- SQL queries must use parameterized queries
- Authentication required for all non-public routes
- All errors must be logged but never expose sensitive data

## Performance Standards
- Database queries must use indexes
- API responses < 200ms for 95% of requests
- Bundle size < 500KB (gzipped)
- Lighthouse score > 90

## Git Workflow
- Branch naming: feature/description, fix/description, chore/description
- Commit messages: Follow conventional commits
- All PRs require code review
- Squash commits before merging

## File Organization
- Components in /components
- API routes in /app/api or /pages/api
- Database models in /lib/db
- Types in /types
- Utils in /lib/utils
- Tests colocated with source files (.test.ts, .spec.ts)

## When Creating New Code
1. Check if similar code exists (DRY principle)
2. Follow established patterns in codebase
3. Add appropriate error handling
4. Include logging for debugging
5. Write tests alongside implementation
6. Update documentation if needed

## Error Handling Pattern
```typescript
try {
  // Operation
} catch (error) {
  logger.error('Context of error', { error, additionalContext });
  throw new AppError('User-friendly message', { cause: error });
}
```

## Component Pattern (React)
```typescript
/**
 * Brief description of component
 * @param props - Component props
 */
export function ComponentName({ prop1, prop2 }: ComponentNameProps) {
  // Hooks first
  // Event handlers
  // Render logic
  
  return (
    // JSX
  );
}
```

## API Endpoint Pattern
```typescript
export async function POST(request: Request) {
  try {
    // 1. Parse and validate input
    // 2. Authenticate and authorize
    // 3. Business logic
    // 4. Return formatted response
  } catch (error) {
    // Error handling
  }
}
```
```

### 5.2 Recommended Cursor Settings
Create `.vscode/settings.json`:

```json
{
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true,
    "source.organizeImports": true
  },
  "typescript.tsdk": "node_modules/typescript/lib",
  "typescript.enablePromptUseWorkspaceTsdk": true,
  "files.exclude": {
    "**/.git": true,
    "**/.next": true,
    "**/node_modules": true,
    "**/.DS_Store": true
  },
  "search.exclude": {
    "**/node_modules": true,
    "**/bower_components": true,
    "**/.next": true,
    "**/dist": true
  },
  "files.associations": {
    "*.css": "tailwindcss"
  },
  "tailwindCSS.experimental.classRegex": [
    ["cva\\(([^)]*)\\)", "[\"'`]([^\"'`]*).*?[\"'`]"],
    ["cx\\(([^)]*)\\)", "(?:'|\"|`)([^']*)(?:'|\"|`)"]
  ]
}
```

### 5.3 Essential Cursor Extensions
Document required extensions in README:

```
Required Extensions:
- ESLint
- Prettier
- Tailwind CSS IntelliSense
- Prisma (if using Prisma)
- Pretty TypeScript Errors
- Error Lens

Recommended Extensions:
- GitLens
- Thunder Client (API testing)
- Database Client (for SQL)
```

---

## 6. Code Quality Standards

### 6.1 TypeScript Configuration
Use strict TypeScript configuration (`tsconfig.json`):

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "lib": ["ES2022", "dom", "dom.iterable"],
    "module": "ESNext",
    "moduleResolution": "bundler",
    "resolveJsonModule": true,
    "allowJs": true,
    "strict": true,
    "noEmit": true,
    "esModuleInterop": true,
    "skipLibCheck": true,
    "forceConsistentCasingInFileNames": true,
    "incremental": true,
    "jsx": "preserve",
    "plugins": [{ "name": "next" }],
    "paths": {
      "@/*": ["./*"]
    },
    
    // Strict checks
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true,
    "noUncheckedIndexedAccess": true,
    "noImplicitReturns": true,
    "noImplicitOverride": true
  },
  "include": ["**/*.ts", "**/*.tsx", ".next/types/**/*.ts"],
  "exclude": ["node_modules"]
}
```

### 6.2 ESLint Configuration
Strict ESLint rules (`.eslintrc.json`):

```json
{
  "extends": [
    "next/core-web-vitals",
    "plugin:@typescript-eslint/recommended",
    "plugin:@typescript-eslint/recommended-requiring-type-checking",
    "prettier"
  ],
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "project": "./tsconfig.json"
  },
  "plugins": ["@typescript-eslint"],
  "rules": {
    "@typescript-eslint/no-unused-vars": ["error", {
      "argsIgnorePattern": "^_",
      "varsIgnorePattern": "^_"
    }],
    "@typescript-eslint/no-explicit-any": "error",
    "@typescript-eslint/explicit-function-return-type": ["warn", {
      "allowExpressions": true
    }],
    "@typescript-eslint/no-floating-promises": "error",
    "@typescript-eslint/await-thenable": "error",
    "no-console": ["warn", { "allow": ["warn", "error"] }]
  }
}
```

### 6.3 Prettier Configuration
Consistent formatting (`.prettierrc`):

```json
{
  "semi": true,
  "trailingComma": "es5",
  "singleQuote": true,
  "printWidth": 80,
  "tabWidth": 2,
  "useTabs": false,
  "arrowParens": "always",
  "endOfLine": "lf",
  "plugins": ["prettier-plugin-tailwindcss"]
}
```

### 6.4 Git Hooks with Husky
Enforce quality before commits (`.husky/pre-commit`):

```bash
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

npm run lint
npm run type-check
npm run test
```

### 6.5 Code Review Checklist
Every PR must pass this checklist:

**Functionality:**
- [ ] Code works as intended
- [ ] All requirements met
- [ ] Edge cases handled
- [ ] Error handling implemented

**Code Quality:**
- [ ] ESLint passes with 0 errors, 0 warnings
- [ ] TypeScript compiles with no errors
- [ ] No 'any' types (unless justified)
- [ ] Functions have JSDoc comments
- [ ] Complex logic is explained with comments
- [ ] No console.log statements (use logger)

**Testing:**
- [ ] Unit tests written and passing
- [ ] Integration tests for API endpoints
- [ ] Test coverage meets 80% threshold
- [ ] Tests are meaningful, not just for coverage

**Security:**
- [ ] No hardcoded secrets
- [ ] User input validated
- [ ] Authentication/authorization checked
- [ ] No SQL injection vulnerabilities
- [ ] XSS prevention implemented

**Performance:**
- [ ] No N+1 query problems
- [ ] Database indexes used appropriately
- [ ] Large lists are paginated
- [ ] Images optimized
- [ ] No unnecessary re-renders (React)

**Documentation:**
- [ ] README updated if needed
- [ ] API documentation updated
- [ ] Complex algorithms documented
- [ ] Breaking changes noted

---

## 7. Error Prevention Framework

### 7.1 Input Validation
**Rule:** Validate all input at the boundary (API layer)

**Implementation:**
```typescript
import { z } from 'zod';

// Define schema
const createUserSchema = z.object({
  email: z.string().email().max(255).toLowerCase().trim(),
  displayName: z.string().min(2).max(100).trim(),
  role: z.enum(['admin', 'manager', 'user']).default('user'),
});

// Validate in API endpoint
export async function POST(request: Request) {
  try {
    const body = await request.json();
    const validatedData = createUserSchema.parse(body);
    
    // Now use validatedData (fully typed and validated)
  } catch (error) {
    if (error instanceof z.ZodError) {
      return Response.json(
        { error: 'VALIDATION_ERROR', fields: error.flatten().fieldErrors },
        { status: 400 }
      );
    }
    throw error;
  }
}
```

### 7.2 Type Safety
**Rule:** Use TypeScript strictly, avoid 'any' type

**Bad:**
```typescript
function processData(data: any) {
  return data.items.map((item: any) => item.value);
}
```

**Good:**
```typescript
interface DataItem {
  id: string;
  value: number;
}

interface Data {
  items: DataItem[];
}

function processData(data: Data): number[] {
  return data.items.map((item) => item.value);
}
```

### 7.3 Error Handling
**Rule:** Never swallow errors, always handle explicitly

**Bad:**
```typescript
try {
  await riskyOperation();
} catch {
  // Silent failure
}
```

**Good:**
```typescript
try {
  await riskyOperation();
} catch (error) {
  logger.error('Risky operation failed', {
    error,
    context: { userId, operation: 'riskyOperation' }
  });
  
  throw new AppError(
    'Unable to complete operation. Please try again.',
    { cause: error }
  );
}
```

### 7.4 Database Safety
**Rule:** Always use parameterized queries, never string concatenation

**Bad:**
```typescript
// SQL injection vulnerability!
const query = `SELECT * FROM users WHERE email = '${email}'`;
```

**Good:**
```typescript
// Using Prisma (parameterized by default)
const user = await prisma.user.findUnique({
  where: { email },
});

// Or with raw SQL (parameterized)
const user = await prisma.$queryRaw`
  SELECT * FROM users WHERE email = ${email}
`;
```

### 7.5 Authentication Safety
**Rule:** Never trust client-side data for authorization

**Bad:**
```typescript
export async function DELETE(request: Request) {
  const { userId } = await request.json();
  // Client says they're this user - trusting them!
  await deleteUser(userId);
}
```

**Good:**
```typescript
export async function DELETE(request: Request) {
  // Get user from verified JWT token
  const session = await getServerSession();
  if (!session) {
    return Response.json({ error: 'Unauthorized' }, { status: 401 });
  }
  
  // Only allow deleting own account (or admin role)
  if (session.user.id !== userId && session.user.role !== 'admin') {
    return Response.json({ error: 'Forbidden' }, { status: 403 });
  }
  
  await deleteUser(userId);
}
```

### 7.6 Environment Variables Safety
**Rule:** Validate environment variables at startup

```typescript
// lib/env.ts
import { z } from 'zod';

const envSchema = z.object({
  DATABASE_URL: z.string().url(),
  JWT_SECRET: z.string().min(32),
  NODE_ENV: z.enum(['development', 'test', 'production']),
  PORT: z.string().transform(Number).pipe(z.number().min(1000).max(65535)),
});

// This will throw if env vars are invalid
export const env = envSchema.parse(process.env);

// Now use env.DATABASE_URL with confidence it's valid
```

---

## 8. Quality Gates & Checkpoints

### 8.1 Pre-Development Gates
✅ **GATE 0:** All items in Pre-Project Checklist completed  
✅ **GATE 1:** Project Specification Document approved  
✅ **GATE 2:** All 10 methodology steps documented  
✅ **GATE 3:** Team has reviewed and understands technical approach  

### 8.2 During Development Gates
✅ **Sprint/Phase Gate:** At end of each sprint/phase:
- [ ] All planned features completed
- [ ] Code reviewed and merged
- [ ] Tests passing with ≥80% coverage
- [ ] Documentation updated
- [ ] Demo completed with stakeholders
- [ ] No critical bugs open

### 8.3 Pre-Deployment Gates
✅ **GATE 4: Code Quality**
- [ ] ESLint: 0 errors, 0 warnings
- [ ] TypeScript: 0 compilation errors
- [ ] Prettier: All files formatted
- [ ] No TODO or FIXME comments in production code

✅ **GATE 5: Testing**
- [ ] Unit tests: ≥80% coverage
- [ ] Integration tests: All critical paths tested
- [ ] E2E tests: All user flows tested
- [ ] Load testing: Meets performance SLAs
- [ ] Security testing: No critical vulnerabilities

✅ **GATE 6: Documentation**
- [ ] README complete and accurate
- [ ] API documentation complete
- [ ] Deployment guide tested
- [ ] Runbook created with common issues

✅ **GATE 7: Security**
- [ ] No hardcoded secrets
- [ ] All inputs validated
- [ ] Authentication/authorization implemented
- [ ] Security audit checklist completed
- [ ] Dependency vulnerability scan passed

✅ **GATE 8: Performance**
- [ ] Lighthouse score ≥90
- [ ] API p95 response time < target
- [ ] Database queries optimized
- [ ] Load testing passed

### 8.4 Production Readiness Checklist
Before deploying to production:

**Infrastructure:**
- [ ] Production environment configured
- [ ] Database backed up and tested
- [ ] SSL certificates installed
- [ ] CDN configured
- [ ] Monitoring and alerting set up
- [ ] Log aggregation configured

**Application:**
- [ ] Environment variables set correctly
- [ ] Database migrations tested
- [ ] Seeds removed or production-safe
- [ ] Error tracking configured (Sentry, etc.)
- [ ] Performance monitoring enabled

**Operations:**
- [ ] Rollback procedure documented and tested
- [ ] Support team trained
- [ ] Incident response plan created
- [ ] On-call rotation established
- [ ] Backup and recovery tested

**Compliance:**
- [ ] Security audit completed
- [ ] Privacy policy updated
- [ ] Terms of service updated
- [ ] Data retention policies implemented
- [ ] Audit logging enabled

---

## 9. AI Agent Utilization

### 9.1 When to Use Cursor AI
**✅ Use Cursor AI for:**
- Boilerplate code generation
- Test case generation
- Documentation generation
- Code refactoring suggestions
- Finding and fixing bugs
- Implementing well-defined patterns
- Code completion and suggestions

**❌ Don't rely solely on Cursor AI for:**
- Architecture decisions
- Security implementation
- Performance optimization
- Complex business logic
- Database schema design
- Critical algorithms

### 9.2 Effective Prompting
**Bad Prompt:**
"Create a user management system"

**Good Prompt:**
```
Create a User CRUD API with the following requirements:

Database Schema (Prisma):
- id: UUID
- email: String (unique, lowercase)
- password: String (hashed with bcrypt)
- role: Enum (admin, user)
- createdAt, updatedAt: DateTime

Endpoints:
1. POST /api/users - Create user (admin only)
2. GET /api/users/:id - Get user (authenticated)
3. PATCH /api/users/:id - Update user (self or admin)
4. DELETE /api/users/:id - Delete user (admin only)

Requirements:
- Use Zod for validation
- Implement proper error handling
- Add authentication middleware
- Include JSDoc comments
- Follow existing patterns in /app/api/auth
```

### 9.3 Code Review with AI
Use Cursor to review code:

```
Review this code for:
1. Security vulnerabilities
2. Performance issues
3. Edge cases not handled
4. TypeScript type safety
5. Best practices violations

[Paste code here]
```

### 9.4 AI-Assisted Testing
Use Cursor to generate comprehensive tests:

```
Generate unit tests for this service:
- Test all public methods
- Include happy path and error cases
- Test edge cases (null, undefined, empty arrays)
- Use Jest and @testing-library conventions
- Aim for 100% coverage of this file

[Paste service code here]
```

---

## 10. Templates & Checklists

### 10.1 Pull Request Template
Create `.github/PULL_REQUEST_TEMPLATE.md`:

```markdown
## Description
[Describe what this PR does]

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Related Issues
Closes #[issue number]

## Testing
- [ ] Unit tests added/updated
- [ ] Integration tests added/updated
- [ ] Manual testing completed

## Checklist
- [ ] Code follows project style guidelines
- [ ] Self-review completed
- [ ] Comments added for complex logic
- [ ] Documentation updated
- [ ] No new warnings or errors
- [ ] Tests pass locally
- [ ] Dependent changes merged

## Screenshots (if applicable)
[Add screenshots]

## Additional Notes
[Any additional context]
```

### 10.2 Bug Report Template
```markdown
## Bug Description
[Clear description of the bug]

## Steps to Reproduce
1. Go to '...'
2. Click on '...'
3. Scroll down to '...'
4. See error

## Expected Behavior
[What should happen]

## Actual Behavior
[What actually happens]

## Environment
- OS: [e.g., macOS 13.0]
- Browser: [e.g., Chrome 118]
- Version: [e.g., 1.2.3]

## Screenshots/Logs
[Add any relevant screenshots or error logs]

## Additional Context
[Any other relevant information]
```

### 10.3 Feature Request Template
```markdown
## Feature Description
[Clear description of the feature]

## Problem it Solves
[What problem does this feature address?]

## Proposed Solution
[How should this feature work?]

## Alternatives Considered
[What other solutions were considered?]

## Additional Context
[Mockups, examples, references]
```

### 10.4 Definition of Done Checklist
For each feature, all items must be checked:

**Development:**
- [ ] Code implemented according to requirements
- [ ] Code follows style guide and conventions
- [ ] No ESLint errors or warnings
- [ ] TypeScript strict mode passes
- [ ] All edge cases handled
- [ ] Error handling implemented
- [ ] Logging added for debugging

**Testing:**
- [ ] Unit tests written and passing
- [ ] Integration tests written and passing
- [ ] E2E tests written and passing (if applicable)
- [ ] Code coverage ≥80%
- [ ] Manual testing completed
- [ ] Tested on target browsers/devices

**Documentation:**
- [ ] Code comments added where needed
- [ ] README updated (if needed)
- [ ] API docs updated (if applicable)
- [ ] Changelog updated

**Review:**
- [ ] Code reviewed by peer
- [ ] All review comments addressed
- [ ] Approved by reviewer

**Deployment:**
- [ ] Merged to main branch
- [ ] Deployed to staging
- [ ] Smoke tested on staging
- [ ] Deployed to production (if ready)
- [ ] Verified in production

---

## Appendix A: Quick Reference

### Common Commands
```bash
# Project setup
npm install
npm run dev

# Code quality
npm run lint
npm run lint:fix
npm run format
npm run type-check

# Testing
npm run test
npm run test:coverage
npm run test:e2e

# Database
npm run db:migrate
npm run db:seed
npm run db:studio

# Build & deploy
npm run build
npm run start
```

### Error Rate Calculation
```
Error Rate = (Number of Errors / Total Transactions) × 100

Target: < 0.2% = < 2 errors per 1000 transactions

Example:
- 10,000 API calls per day
- Maximum allowed errors: 20 per day
- Implement monitoring to alert at 15 errors/day (75% threshold)
```

### Key Metrics to Track
- **Code Quality:** ESLint errors/warnings count
- **Test Coverage:** Percentage of code covered by tests
- **Performance:** API response times (p50, p95, p99)
- **Errors:** Production error rate
- **Security:** Vulnerability count from scans
- **Documentation:** % of public APIs documented

---

## Appendix B: Recommended Reading

### Books
- "Clean Code" by Robert C. Martin
- "The Pragmatic Programmer" by Hunt & Thomas
- "Designing Data-Intensive Applications" by Martin Kleppmann

### Online Resources
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [Next.js Documentation](https://nextjs.org/docs)
- [React Best Practices](https://react.dev/)
- [Prisma Guides](https://www.prisma.io/docs)
- [OWASP Top 10](https://owasp.org/www-project-top-ten/)

---

## Document Control

**Revision History:**
- v1.0 (2025-11-10): Initial version

**Next Review Date:** 2025-12-10

**Feedback:** Submit improvements via GitHub issues or email

---

*This document is a living standard. As we learn and improve, we update this guide to reflect best practices.*
