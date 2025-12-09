# 🛡️ VirusTotal SaaS Platform

<div align="center">

![Platform](https://img.shields.io/badge/Platform-SaaS-blue.svg)
![Status](https://img.shields.io/badge/Status-Production_Ready-success.svg)
![License](https://img.shields.io/badge/License-MIT-green.svg)

**Enterprise-grade SaaS platform for VirusTotal scanning with user authentication, subscriptions, team collaboration, and comprehensive scan management**

[Features](#-features) • [Tech Stack](#-tech-stack) • [Installation](#-installation) • [Documentation](#-documentation) • [Demo](#-demo)

</div>

---

## ✨ Features

### 🔐 User Management
- **User Registration & Login** - Secure authentication with JWT tokens
- **Email Verification** - Verify user emails before activation
- **Password Reset** - Secure password recovery flow
- **User Profiles** - Customizable user profiles with avatars
- **Role-Based Access Control** - Admin, Team Lead, Member roles
- **Two-Factor Authentication** - Optional 2FA for enhanced security

### 💳 Subscription Management
- **Multiple Pricing Tiers**:
  - **Free Tier**: 10 scans/month, basic features
  - **Pro Tier**: 500 scans/month, advanced features, priority support
  - **Enterprise Tier**: Unlimited scans, team collaboration, API access, dedicated support
- **Stripe Integration** - Secure payment processing
- **Subscription Management** - Upgrade, downgrade, cancel anytime
- **Usage Tracking** - Real-time scan quota monitoring
- **Billing History** - Complete invoice and payment history

### 🔍 Scanning Features
- **Multi-Target Scanning**:
  - File uploads (up to 100MB)
  - URL scanning
  - Domain reputation checks
  - IP address analysis
- **Batch Scanning** - Upload and scan multiple files at once
- **Scheduled Scans** - Automate recurring scans
- **Scan History** - Complete history with filtering and search
- **Real-time Results** - Live scan progress updates
- **Detailed Reports** - Comprehensive threat analysis reports
- **Export Options** - Download reports as PDF, JSON, CSV

### 👥 Team Collaboration
- **Team Creation** - Create and manage multiple teams
- **Member Invitations** - Invite team members via email
- **Role Management** - Assign roles (Owner, Admin, Member)
- **Shared Scans** - Share scan results with team members
- **Team Dashboard** - Centralized view of team activity
- **Audit Logs** - Track all team actions and changes

### 📊 Analytics & Reporting
- **Dashboard Analytics**:
  - Total scans performed
  - Threat detection rate
  - Scan history trends
  - Usage statistics
- **Custom Reports** - Generate custom security reports
- **Export Analytics** - Download analytics data
- **Threat Intelligence** - Aggregated threat insights
- **Compliance Reports** - Generate compliance documentation

### 🔌 API Access
- **RESTful API** - Complete API for programmatic access
- **API Key Management** - Generate and manage API keys
- **Rate Limiting** - Tier-based rate limits
- **API Documentation** - Interactive Swagger/OpenAPI docs
- **Webhooks** - Real-time notifications for scan completion

### 🔔 Notifications
- **Email Notifications** - Scan completion, threats detected
- **In-App Notifications** - Real-time alerts
- **Webhook Integration** - Custom webhook endpoints
- **Slack Integration** - Send alerts to Slack channels
- **Custom Alerts** - Configure custom notification rules

### 🛡️ Security Features
- **Encrypted Storage** - All data encrypted at rest
- **Secure File Handling** - Sandboxed file processing
- **Rate Limiting** - Prevent abuse and DDoS
- **CORS Protection** - Secure cross-origin requests
- **SQL Injection Prevention** - Parameterized queries
- **XSS Protection** - Input sanitization
- **CSRF Protection** - Token-based CSRF prevention

---

## 🏗️ Tech Stack

### Frontend
```
React.js 18          - Modern UI framework
TypeScript           - Type-safe JavaScript
Tailwind CSS         - Utility-first CSS framework
Shadcn/ui            - Beautiful UI components
React Query          - Data fetching and caching
React Router         - Client-side routing
Zustand              - State management
Axios                - HTTP client
Chart.js             - Data visualization
React Dropzone       - File upload handling
```

### Backend
```
Node.js 18+          - JavaScript runtime
Express.js           - Web framework
TypeScript           - Type-safe backend
PostgreSQL           - Primary database
Redis                - Caching and sessions
Prisma ORM           - Database ORM
JWT                  - Authentication tokens
Bcrypt               - Password hashing
Multer               - File upload handling
Bull                 - Job queue for async tasks
```

### Infrastructure
```
Docker               - Containerization
Docker Compose       - Multi-container orchestration
Nginx                - Reverse proxy
PM2                  - Process management
AWS S3               - File storage
AWS CloudFront       - CDN
AWS RDS              - Managed PostgreSQL
AWS ElastiCache      - Managed Redis
```

### Payment & Email
```
Stripe               - Payment processing
SendGrid             - Transactional emails
Twilio               - SMS notifications (optional)
```

### Monitoring & Analytics
```
Sentry               - Error tracking
LogRocket            - Session replay
Google Analytics     - Usage analytics
Prometheus           - Metrics collection
Grafana              - Metrics visualization
```

---

## 📁 Project Structure

```
virustotal-saas-platform/
├── frontend/                    # React frontend
│   ├── src/
│   │   ├── components/         # Reusable components
│   │   │   ├── auth/          # Authentication components
│   │   │   ├── dashboard/     # Dashboard components
│   │   │   ├── scanning/      # Scan components
│   │   │   ├── teams/         # Team management
│   │   │   ├── billing/       # Subscription & billing
│   │   │   └── common/        # Shared components
│   │   ├── pages/             # Page components
│   │   ├── hooks/             # Custom React hooks
│   │   ├── services/          # API services
│   │   ├── store/             # State management
│   │   ├── utils/             # Utility functions
│   │   ├── types/             # TypeScript types
│   │   └── App.tsx            # Main app component
│   ├── public/                # Static assets
│   └── package.json
│
├── backend/                     # Node.js backend
│   ├── src/
│   │   ├── controllers/       # Route controllers
│   │   │   ├── auth.controller.ts
│   │   │   ├── user.controller.ts
│   │   │   ├── scan.controller.ts
│   │   │   ├── team.controller.ts
│   │   │   ├── subscription.controller.ts
│   │   │   └── api.controller.ts
│   │   ├── services/          # Business logic
│   │   │   ├── auth.service.ts
│   │   │   ├── virustotal.service.ts
│   │   │   ├── stripe.service.ts
│   │   │   ├── email.service.ts
│   │   │   └── notification.service.ts
│   │   ├── models/            # Database models (Prisma)
│   │   ├── middleware/        # Express middleware
│   │   │   ├── auth.middleware.ts
│   │   │   ├── rateLimit.middleware.ts
│   │   │   └── validation.middleware.ts
│   │   ├── routes/            # API routes
│   │   ├── utils/             # Utility functions
│   │   ├── config/            # Configuration
│   │   ├── jobs/              # Background jobs
│   │   └── server.ts          # Express server
│   ├── prisma/                # Prisma schema
│   │   └── schema.prisma
│   └── package.json
│
├── docker/                      # Docker configurations
│   ├── frontend.Dockerfile
│   ├── backend.Dockerfile
│   └── nginx.conf
│
├── docker-compose.yml          # Docker Compose config
├── .env.example               # Environment variables template
└── README.md                  # This file
```

---

## 🚀 Installation

### Prerequisites
- Node.js 18+ and npm/yarn
- PostgreSQL 14+
- Redis 6+
- Docker & Docker Compose (optional)
- VirusTotal API key
- Stripe account (for payments)
- SendGrid account (for emails)

### Option 1: Docker (Recommended)

```bash
# Clone the repository
git clone https://github.com/Hemant617/virustotal-saas-platform.git
cd virustotal-saas-platform

# Copy environment variables
cp .env.example .env

# Edit .env with your credentials
nano .env

# Start all services with Docker Compose
docker-compose up -d

# Run database migrations
docker-compose exec backend npm run migrate

# Access the application
# Frontend: http://localhost:3000
# Backend API: http://localhost:5000
# Admin Panel: http://localhost:3000/admin
```

### Option 2: Manual Installation

#### Backend Setup

```bash
# Navigate to backend
cd backend

# Install dependencies
npm install

# Copy environment variables
cp .env.example .env

# Edit .env with your credentials
nano .env

# Run database migrations
npx prisma migrate dev

# Seed initial data (optional)
npm run seed

# Start development server
npm run dev

# Backend runs on http://localhost:5000
```

#### Frontend Setup

```bash
# Navigate to frontend
cd frontend

# Install dependencies
npm install

# Copy environment variables
cp .env.example .env

# Edit .env with backend API URL
nano .env

# Start development server
npm run dev

# Frontend runs on http://localhost:3000
```

---

## ⚙️ Configuration

### Environment Variables

#### Backend (.env)
```env
# Server
NODE_ENV=development
PORT=5000
API_URL=http://localhost:5000

# Database
DATABASE_URL=postgresql://user:password@localhost:5432/virustotal_saas

# Redis
REDIS_URL=redis://localhost:6379

# JWT
JWT_SECRET=your-super-secret-jwt-key
JWT_EXPIRES_IN=7d

# VirusTotal
VIRUSTOTAL_API_KEY=your-virustotal-api-key

# Stripe
STRIPE_SECRET_KEY=sk_test_...
STRIPE_WEBHOOK_SECRET=whsec_...
STRIPE_PRICE_ID_FREE=price_...
STRIPE_PRICE_ID_PRO=price_...
STRIPE_PRICE_ID_ENTERPRISE=price_...

# SendGrid
SENDGRID_API_KEY=SG...
SENDGRID_FROM_EMAIL=noreply@yourdomain.com

# AWS S3 (for file storage)
AWS_ACCESS_KEY_ID=your-access-key
AWS_SECRET_ACCESS_KEY=your-secret-key
AWS_S3_BUCKET=virustotal-scans
AWS_REGION=us-east-1

# Frontend URL
FRONTEND_URL=http://localhost:3000

# Sentry (optional)
SENTRY_DSN=https://...
```

#### Frontend (.env)
```env
REACT_APP_API_URL=http://localhost:5000
REACT_APP_STRIPE_PUBLIC_KEY=pk_test_...
REACT_APP_GA_TRACKING_ID=UA-...
```

---

## 📊 Database Schema

### Core Tables

```sql
-- Users
users (
  id, email, password_hash, name, avatar_url,
  email_verified, two_factor_enabled, role,
  subscription_tier, subscription_status,
  created_at, updated_at
)

-- Subscriptions
subscriptions (
  id, user_id, stripe_customer_id, stripe_subscription_id,
  plan, status, current_period_start, current_period_end,
  cancel_at_period_end, created_at, updated_at
)

-- Scans
scans (
  id, user_id, team_id, scan_type, target,
  file_name, file_size, file_hash, status,
  virustotal_scan_id, result, threat_level,
  detections, total_engines, created_at, completed_at
)

-- Teams
teams (
  id, name, owner_id, subscription_tier,
  created_at, updated_at
)

-- Team Members
team_members (
  id, team_id, user_id, role, invited_by,
  joined_at, created_at
)

-- API Keys
api_keys (
  id, user_id, key_hash, name, last_used_at,
  expires_at, created_at
)

-- Notifications
notifications (
  id, user_id, type, title, message, read,
  created_at
)

-- Audit Logs
audit_logs (
  id, user_id, team_id, action, resource_type,
  resource_id, metadata, ip_address, created_at
)
```

---

## 🎨 User Interface

### Pages

1. **Landing Page** (`/`)
   - Hero section with value proposition
   - Features showcase
   - Pricing table
   - Testimonials
   - CTA buttons

2. **Authentication** (`/auth`)
   - Login (`/auth/login`)
   - Register (`/auth/register`)
   - Forgot Password (`/auth/forgot-password`)
   - Reset Password (`/auth/reset-password`)
   - Email Verification (`/auth/verify-email`)

3. **Dashboard** (`/dashboard`)
   - Overview statistics
   - Recent scans
   - Usage metrics
   - Quick actions

4. **Scanning** (`/scan`)
   - File Upload (`/scan/file`)
   - URL Scan (`/scan/url`)
   - Domain Scan (`/scan/domain`)
   - IP Scan (`/scan/ip`)
   - Batch Scan (`/scan/batch`)

5. **Scan History** (`/scans`)
   - List view with filters
   - Search functionality
   - Export options
   - Detailed scan view (`/scans/:id`)

6. **Teams** (`/teams`)
   - Team list (`/teams`)
   - Create team (`/teams/create`)
   - Team dashboard (`/teams/:id`)
   - Member management (`/teams/:id/members`)
   - Team settings (`/teams/:id/settings`)

7. **Billing** (`/billing`)
   - Current plan
   - Usage statistics
   - Upgrade/downgrade
   - Payment methods
   - Billing history
   - Invoices

8. **Settings** (`/settings`)
   - Profile (`/settings/profile`)
   - Security (`/settings/security`)
   - Notifications (`/settings/notifications`)
   - API Keys (`/settings/api`)
   - Integrations (`/settings/integrations`)

9. **Admin Panel** (`/admin`)
   - User management
   - Subscription overview
   - System statistics
   - Audit logs

---

## 🔌 API Documentation

### Authentication Endpoints

```
POST   /api/auth/register          - Register new user
POST   /api/auth/login             - Login user
POST   /api/auth/logout            - Logout user
POST   /api/auth/refresh           - Refresh JWT token
POST   /api/auth/forgot-password   - Request password reset
POST   /api/auth/reset-password    - Reset password
POST   /api/auth/verify-email      - Verify email address
POST   /api/auth/resend-verification - Resend verification email
```

### User Endpoints

```
GET    /api/users/me               - Get current user
PUT    /api/users/me               - Update current user
DELETE /api/users/me               - Delete account
PUT    /api/users/me/password      - Change password
POST   /api/users/me/avatar        - Upload avatar
GET    /api/users/me/usage         - Get usage statistics
```

### Scan Endpoints

```
POST   /api/scans/file             - Scan file
POST   /api/scans/url              - Scan URL
POST   /api/scans/domain           - Scan domain
POST   /api/scans/ip               - Scan IP address
POST   /api/scans/batch            - Batch scan
GET    /api/scans                  - List scans
GET    /api/scans/:id              - Get scan details
DELETE /api/scans/:id              - Delete scan
GET    /api/scans/:id/report       - Download report
POST   /api/scans/:id/rescan       - Rescan target
```

### Team Endpoints

```
POST   /api/teams                  - Create team
GET    /api/teams                  - List teams
GET    /api/teams/:id              - Get team details
PUT    /api/teams/:id              - Update team
DELETE /api/teams/:id              - Delete team
POST   /api/teams/:id/members      - Invite member
GET    /api/teams/:id/members      - List members
PUT    /api/teams/:id/members/:userId - Update member role
DELETE /api/teams/:id/members/:userId - Remove member
GET    /api/teams/:id/scans        - List team scans
```

### Subscription Endpoints

```
GET    /api/subscriptions/plans    - List available plans
POST   /api/subscriptions/checkout - Create checkout session
POST   /api/subscriptions/portal   - Create customer portal session
GET    /api/subscriptions/current  - Get current subscription
POST   /api/subscriptions/upgrade  - Upgrade subscription
POST   /api/subscriptions/cancel   - Cancel subscription
GET    /api/subscriptions/invoices - List invoices
```

### API Key Endpoints

```
POST   /api/api-keys               - Create API key
GET    /api/api-keys               - List API keys
DELETE /api/api-keys/:id           - Delete API key
PUT    /api/api-keys/:id           - Update API key
```

---

## 💳 Pricing Tiers

### Free Tier
- **Price**: $0/month
- **Features**:
  - 10 scans per month
  - File, URL, domain, IP scanning
  - Basic scan reports
  - 7-day scan history
  - Community support

### Pro Tier
- **Price**: $29/month
- **Features**:
  - 500 scans per month
  - All Free features
  - Advanced reports (PDF export)
  - 90-day scan history
  - Scheduled scans
  - Priority support
  - API access (1,000 requests/day)

### Enterprise Tier
- **Price**: $199/month
- **Features**:
  - Unlimited scans
  - All Pro features
  - Team collaboration (up to 10 members)
  - Unlimited scan history
  - Custom integrations
  - Webhooks
  - Dedicated support
  - API access (unlimited)
  - White-label options
  - SLA guarantee

---

## 🔒 Security Best Practices

### Implemented Security Measures

1. **Authentication & Authorization**
   - JWT-based authentication
   - Secure password hashing (bcrypt)
   - Role-based access control (RBAC)
   - Session management with Redis
   - Optional 2FA

2. **Data Protection**
   - Encryption at rest (database)
   - Encryption in transit (HTTPS/TLS)
   - Secure file storage (AWS S3)
   - PII data protection

3. **API Security**
   - Rate limiting per tier
   - CORS configuration
   - Input validation
   - SQL injection prevention
   - XSS protection
   - CSRF tokens

4. **Infrastructure Security**
   - Docker containerization
   - Environment variable management
   - Secrets management
   - Regular security updates
   - Automated backups

5. **Monitoring & Logging**
   - Error tracking (Sentry)
   - Audit logs
   - Security event logging
   - Anomaly detection

---

## 📈 Deployment

### Production Deployment (AWS)

```bash
# 1. Set up AWS infrastructure
# - RDS PostgreSQL instance
# - ElastiCache Redis instance
# - S3 bucket for file storage
# - CloudFront CDN
# - EC2 instances or ECS for containers

# 2. Configure environment variables
# Set all production environment variables

# 3. Build Docker images
docker build -f docker/frontend.Dockerfile -t virustotal-frontend .
docker build -f docker/backend.Dockerfile -t virustotal-backend .

# 4. Push to container registry
docker tag virustotal-frontend:latest your-registry/virustotal-frontend:latest
docker push your-registry/virustotal-frontend:latest

# 5. Deploy with Docker Compose or Kubernetes
docker-compose -f docker-compose.prod.yml up -d

# 6. Run database migrations
docker-compose exec backend npm run migrate:prod

# 7. Set up SSL certificates (Let's Encrypt)
certbot --nginx -d yourdomain.com -d www.yourdomain.com
```

### Alternative Deployment Options

**Vercel (Frontend)**
```bash
cd frontend
vercel --prod
```

**Railway (Backend)**
```bash
cd backend
railway up
```

**Heroku (Full Stack)**
```bash
heroku create virustotal-saas
git push heroku main
```

---

## 🧪 Testing

```bash
# Backend tests
cd backend
npm run test              # Run all tests
npm run test:unit         # Unit tests
npm run test:integration  # Integration tests
npm run test:e2e          # End-to-end tests
npm run test:coverage     # Coverage report

# Frontend tests
cd frontend
npm run test              # Run all tests
npm run test:watch        # Watch mode
npm run test:coverage     # Coverage report
```

---

## 📚 Documentation

- **API Documentation**: http://localhost:5000/api-docs (Swagger UI)
- **User Guide**: `/docs/user-guide.md`
- **Admin Guide**: `/docs/admin-guide.md`
- **Developer Guide**: `/docs/developer-guide.md`
- **Deployment Guide**: `/docs/deployment.md`

---

## 🤝 Contributing

We welcome contributions! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for details.

---

## 📝 License

This project is licensed under the MIT License - see [LICENSE](LICENSE) file for details.

---

## 👨‍💻 Author

**Hemant Kaushal**

- 📧 Email: hemuh877@gmail.com
- 💼 LinkedIn: [linkedin.com/in/hemantkaushal](https://linkedin.com/in/hemantkaushal)
- 💻 GitHub: [@Hemant617](https://github.com/Hemant617)

---

## 🙏 Acknowledgments

- VirusTotal for their excellent API
- Stripe for payment processing
- All open-source contributors

---

<div align="center">

**⭐ Star this repository if you find it useful!**

**Made with ❤️ by [Hemant Kaushal](https://github.com/Hemant617)**

</div>
