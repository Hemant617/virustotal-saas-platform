# 🚀 Deployment Guide

Complete guide for deploying the VirusTotal SaaS Platform to production.

---

## 📋 Table of Contents

1. [Prerequisites](#prerequisites)
2. [Local Development](#local-development)
3. [Production Deployment](#production-deployment)
4. [Platform-Specific Guides](#platform-specific-guides)
5. [Post-Deployment](#post-deployment)
6. [Monitoring & Maintenance](#monitoring--maintenance)

---

## Prerequisites

### Required Accounts & Services

1. **VirusTotal API Key**
   - Sign up at https://www.virustotal.com/
   - Get your API key from https://www.virustotal.com/gui/my-apikey

2. **Stripe Account**
   - Create account at https://stripe.com/
   - Get API keys from Dashboard
   - Create products and prices for each tier

3. **SendGrid Account**
   - Sign up at https://sendgrid.com/
   - Verify sender email
   - Get API key

4. **AWS Account** (for S3 storage)
   - Create S3 bucket
   - Get access keys
   - Configure bucket permissions

5. **Domain Name**
   - Purchase domain (e.g., from Namecheap, GoDaddy)
   - Configure DNS settings

---

## Local Development

### 1. Clone Repository

```bash
git clone https://github.com/Hemant617/virustotal-saas-platform.git
cd virustotal-saas-platform
```

### 2. Environment Setup

```bash
# Copy environment template
cp .env.example .env

# Edit with your credentials
nano .env
```

### 3. Start with Docker Compose

```bash
# Start all services
docker-compose up -d

# View logs
docker-compose logs -f

# Stop services
docker-compose down
```

### 4. Manual Setup (Without Docker)

#### Backend

```bash
cd backend

# Install dependencies
npm install

# Run migrations
npx prisma migrate dev

# Seed database (optional)
npm run seed

# Start development server
npm run dev
```

#### Frontend

```bash
cd frontend

# Install dependencies
npm install

# Start development server
npm run dev
```

---

## Production Deployment

### Option 1: AWS Deployment (Recommended)

#### Step 1: Set Up AWS Infrastructure

**1. RDS PostgreSQL**
```bash
# Create RDS instance
aws rds create-db-instance \
  --db-instance-identifier virustotal-db \
  --db-instance-class db.t3.micro \
  --engine postgres \
  --master-username admin \
  --master-user-password YourPassword123 \
  --allocated-storage 20
```

**2. ElastiCache Redis**
```bash
# Create Redis cluster
aws elasticache create-cache-cluster \
  --cache-cluster-id virustotal-redis \
  --cache-node-type cache.t3.micro \
  --engine redis \
  --num-cache-nodes 1
```

**3. S3 Bucket**
```bash
# Create S3 bucket
aws s3 mb s3://virustotal-scans

# Configure CORS
aws s3api put-bucket-cors \
  --bucket virustotal-scans \
  --cors-configuration file://s3-cors.json
```

**4. EC2 Instances**
```bash
# Launch EC2 instance
aws ec2 run-instances \
  --image-id ami-0c55b159cbfafe1f0 \
  --instance-type t3.medium \
  --key-name your-key-pair \
  --security-group-ids sg-xxxxxxxx
```

#### Step 2: Deploy Application

**1. SSH into EC2**
```bash
ssh -i your-key.pem ubuntu@your-ec2-ip
```

**2. Install Dependencies**
```bash
# Update system
sudo apt update && sudo apt upgrade -y

# Install Node.js
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs

# Install Docker
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# Install Docker Compose
sudo curl -L "https://github.com/docker/compose/releases/download/v2.23.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

**3. Clone and Configure**
```bash
# Clone repository
git clone https://github.com/Hemant617/virustotal-saas-platform.git
cd virustotal-saas-platform

# Set environment variables
nano .env
# Add production values

# Build and start
docker-compose -f docker-compose.prod.yml up -d
```

**4. Set Up SSL with Let's Encrypt**
```bash
# Install Certbot
sudo apt install certbot python3-certbot-nginx

# Get SSL certificate
sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com

# Auto-renewal
sudo certbot renew --dry-run
```

---

### Option 2: Railway Deployment

#### Backend Deployment

```bash
# Install Railway CLI
npm install -g @railway/cli

# Login
railway login

# Initialize project
cd backend
railway init

# Add PostgreSQL
railway add postgresql

# Add Redis
railway add redis

# Set environment variables
railway variables set VIRUSTOTAL_API_KEY=your_key
railway variables set STRIPE_SECRET_KEY=your_key
# ... set all other variables

# Deploy
railway up
```

#### Frontend Deployment (Vercel)

```bash
# Install Vercel CLI
npm install -g vercel

# Deploy
cd frontend
vercel --prod

# Set environment variables in Vercel dashboard
```

---

### Option 3: Heroku Deployment

#### Backend

```bash
# Install Heroku CLI
curl https://cli-assets.heroku.com/install.sh | sh

# Login
heroku login

# Create app
heroku create virustotal-backend

# Add PostgreSQL
heroku addons:create heroku-postgresql:hobby-dev

# Add Redis
heroku addons:create heroku-redis:hobby-dev

# Set environment variables
heroku config:set VIRUSTOTAL_API_KEY=your_key
heroku config:set STRIPE_SECRET_KEY=your_key
# ... set all variables

# Deploy
git push heroku main

# Run migrations
heroku run npm run migrate:prod
```

#### Frontend (Vercel)

Same as Railway option above.

---

### Option 4: DigitalOcean App Platform

#### 1. Create App

```bash
# Install doctl
snap install doctl

# Authenticate
doctl auth init

# Create app
doctl apps create --spec .do/app.yaml
```

#### 2. Configure Database

```bash
# Create managed PostgreSQL
doctl databases create virustotal-db \
  --engine pg \
  --region nyc1 \
  --size db-s-1vcpu-1gb

# Create managed Redis
doctl databases create virustotal-redis \
  --engine redis \
  --region nyc1 \
  --size db-s-1vcpu-1gb
```

---

## Platform-Specific Guides

### Vercel (Frontend Only)

**vercel.json**
```json
{
  "buildCommand": "npm run build",
  "outputDirectory": "dist",
  "devCommand": "npm run dev",
  "installCommand": "npm install",
  "framework": "vite",
  "env": {
    "REACT_APP_API_URL": "@api_url"
  }
}
```

**Deploy**
```bash
vercel --prod
```

---

### Netlify (Frontend Only)

**netlify.toml**
```toml
[build]
  command = "npm run build"
  publish = "dist"

[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200

[build.environment]
  REACT_APP_API_URL = "https://api.yourdomain.com"
```

**Deploy**
```bash
netlify deploy --prod
```

---

## Post-Deployment

### 1. Database Setup

```bash
# Run migrations
npm run migrate:prod

# Seed initial data
npm run seed
```

### 2. Stripe Webhook Configuration

```bash
# Get webhook signing secret
stripe listen --forward-to https://api.yourdomain.com/webhooks/stripe

# Add to environment variables
STRIPE_WEBHOOK_SECRET=whsec_...
```

### 3. DNS Configuration

**A Records**
```
@ -> Your-Server-IP
www -> Your-Server-IP
api -> Your-Server-IP
```

**CNAME Records**
```
www -> yourdomain.com
```

### 4. SSL Certificate

```bash
# Let's Encrypt (Free)
sudo certbot --nginx -d yourdomain.com -d www.yourdomain.com

# Or use Cloudflare (Free SSL + CDN)
```

### 5. Email Configuration

**SendGrid Domain Authentication**
1. Go to SendGrid Dashboard
2. Settings → Sender Authentication
3. Authenticate your domain
4. Add DNS records

---

## Monitoring & Maintenance

### 1. Set Up Monitoring

**Sentry (Error Tracking)**
```bash
# Install Sentry
npm install @sentry/node @sentry/react

# Configure in code
Sentry.init({
  dsn: process.env.SENTRY_DSN,
  environment: process.env.NODE_ENV
});
```

**Uptime Monitoring**
- UptimeRobot (free)
- Pingdom
- StatusCake

### 2. Logging

**Winston (Backend)**
```javascript
const winston = require('winston');

const logger = winston.createLogger({
  level: 'info',
  format: winston.format.json(),
  transports: [
    new winston.transports.File({ filename: 'error.log', level: 'error' }),
    new winston.transports.File({ filename: 'combined.log' })
  ]
});
```

### 3. Backups

**Database Backups**
```bash
# Automated daily backups
0 2 * * * pg_dump -U postgres virustotal_saas > /backups/db_$(date +\%Y\%m\%d).sql
```

**S3 Versioning**
```bash
aws s3api put-bucket-versioning \
  --bucket virustotal-scans \
  --versioning-configuration Status=Enabled
```

### 4. Performance Monitoring

**New Relic**
```bash
npm install newrelic
```

**Google Analytics**
```javascript
// Add to frontend
ReactGA.initialize('UA-XXXXXXXXX-X');
```

---

## Security Checklist

- [ ] SSL/TLS certificate installed
- [ ] Environment variables secured
- [ ] Database credentials rotated
- [ ] API keys secured
- [ ] CORS configured properly
- [ ] Rate limiting enabled
- [ ] Input validation implemented
- [ ] SQL injection prevention
- [ ] XSS protection enabled
- [ ] CSRF tokens implemented
- [ ] Security headers configured
- [ ] Regular security updates
- [ ] Backup strategy in place
- [ ] Monitoring and alerts configured

---

## Troubleshooting

### Common Issues

**1. Database Connection Failed**
```bash
# Check connection string
echo $DATABASE_URL

# Test connection
psql $DATABASE_URL
```

**2. Redis Connection Failed**
```bash
# Check Redis
redis-cli ping

# Check connection string
echo $REDIS_URL
```

**3. File Upload Issues**
```bash
# Check S3 permissions
aws s3api get-bucket-acl --bucket virustotal-scans

# Check file size limits
# Nginx: client_max_body_size 100M;
```

**4. Stripe Webhook Not Working**
```bash
# Test webhook
stripe trigger payment_intent.succeeded

# Check webhook secret
echo $STRIPE_WEBHOOK_SECRET
```

---

## Scaling

### Horizontal Scaling

**Load Balancer**
```bash
# AWS Application Load Balancer
aws elbv2 create-load-balancer \
  --name virustotal-lb \
  --subnets subnet-xxx subnet-yyy
```

**Auto Scaling**
```bash
# Create Auto Scaling Group
aws autoscaling create-auto-scaling-group \
  --auto-scaling-group-name virustotal-asg \
  --min-size 2 \
  --max-size 10 \
  --desired-capacity 2
```

### Database Scaling

**Read Replicas**
```bash
# Create read replica
aws rds create-db-instance-read-replica \
  --db-instance-identifier virustotal-db-replica \
  --source-db-instance-identifier virustotal-db
```

---

## Cost Optimization

### AWS Cost Estimates

**Small Scale (< 1000 users)**
- EC2 t3.small: $15/month
- RDS db.t3.micro: $15/month
- ElastiCache t3.micro: $12/month
- S3 storage: $5/month
- **Total: ~$50/month**

**Medium Scale (1000-10000 users)**
- EC2 t3.medium x2: $60/month
- RDS db.t3.small: $30/month
- ElastiCache t3.small: $25/month
- S3 storage: $20/month
- Load Balancer: $20/month
- **Total: ~$155/month**

**Large Scale (10000+ users)**
- EC2 t3.large x4: $240/month
- RDS db.m5.large: $150/month
- ElastiCache m5.large: $100/month
- S3 storage: $50/month
- Load Balancer: $20/month
- CloudFront CDN: $50/month
- **Total: ~$610/month**

---

## Support

For deployment issues:
- **Email**: hemuh877@gmail.com
- **GitHub Issues**: https://github.com/Hemant617/virustotal-saas-platform/issues
- **Documentation**: https://github.com/Hemant617/virustotal-saas-platform

---

<div align="center">

**Last Updated**: December 2025

**Made with ❤️ by [Hemant Kaushal](https://github.com/Hemant617)**

</div>
