# 🚀 AfWeather Backend Deployment Guide

## 📋 Pre-Deployment Checklist

- [ ] All environment variables configured
- [ ] Database migrations tested
- [ ] API keys obtained (Weather, Payment providers)
- [ ] SSL certificate ready (for production)
- [ ] Domain name configured (optional)
- [ ] Backup strategy planned

---

## 🎯 Deployment Option 1: Railway.app (RECOMMENDED FOR MVP)

**Best for:** Quick deployment, managed infrastructure, auto-scaling  
**Cost:** Free tier, then $5-20/month  
**Time:** 15-30 minutes

### Step 1: Prepare Your Code

1. Ensure your code is in a Git repository
2. Push to GitHub:
```bash
cd C:\Users\Admin\CascadeProjects\WeatherAfrica\backend
git init
git add .
git commit -m "Initial backend commit"
git remote add origin https://github.com/YOUR_USERNAME/afweather-backend.git
git push -u origin main
```

### Step 2: Create Railway Account

1. Go to https://railway.app
2. Sign up with GitHub
3. Verify your email

### Step 3: Create New Project

1. Click "New Project"
2. Select "Deploy from GitHub repo"
3. Choose your `afweather-backend` repository
4. Railway will auto-detect Node.js

### Step 4: Add PostgreSQL

1. Click "New" → "Database" → "Add PostgreSQL"
2. Railway will provision a managed PostgreSQL instance
3. Note the connection details (auto-populated in environment)

### Step 5: Add Redis

1. Click "New" → "Database" → "Add Redis"
2. Railway will provision a managed Redis instance
3. Connection details auto-populated

### Step 6: Configure Environment Variables

1. Click on your backend service
2. Go to "Variables" tab
3. Add these variables:

```
NODE_ENV=production
PORT=3000

# Database (auto-populated by Railway)
DATABASE_URL=${{Postgres.DATABASE_URL}}

# Redis (auto-populated by Railway)
REDIS_URL=${{Redis.REDIS_URL}}

# JWT Secrets (CHANGE THESE!)
JWT_SECRET=your_super_secure_jwt_secret_CHANGE_THIS
JWT_REFRESH_SECRET=your_super_secure_refresh_secret_CHANGE_THIS

# Weather APIs
OPENWEATHER_API_KEY=your_openweathermap_key
WEATHERAPI_KEY=your_weatherapi_key
VISUALCROSSING_API_KEY=your_visualcrossing_key
DEFAULT_WEATHER_PROVIDER=openweathermap

# Payment Providers
STRIPE_SECRET_KEY=sk_live_your_stripe_key
MPESA_CONSUMER_KEY=your_mpesa_key
MPESA_CONSUMER_SECRET=your_mpesa_secret
PAYSTACK_SECRET_KEY=sk_live_your_paystack_key

# CORS
CORS_ORIGIN=*
```

### Step 7: Deploy

1. Railway will automatically deploy
2. Wait for build to complete (2-5 minutes)
3. Get your deployment URL (e.g., `https://afweather-backend.up.railway.app`)

### Step 8: Test Your API

```bash
curl https://your-app.up.railway.app/health
```

Should return:
```json
{
  "success": true,
  "message": "AfWeather Backend API is running",
  "version": "v1",
  "timestamp": "2025-10-14T..."
}
```

### Step 9: Update Frontend

Update your React Native `.env`:
```
BACKEND_API_URL=https://your-app.up.railway.app
```

---

## 🎯 Deployment Option 2: Render.com

**Best for:** Free tier, automatic SSL, easy setup  
**Cost:** Free tier, then $7-25/month  
**Time:** 20-30 minutes

### Step 1: Create Render Account

1. Go to https://render.com
2. Sign up with GitHub
3. Verify email

### Step 2: Create PostgreSQL Database

1. Click "New +" → "PostgreSQL"
2. Name: `afweather-db`
3. Database: `afweather`
4. User: `afweather_user`
5. Region: Choose closest to your users
6. Plan: Free (or Starter $7/month)
7. Click "Create Database"
8. Copy the "Internal Database URL"

### Step 3: Create Redis Instance

1. Click "New +" → "Redis"
2. Name: `afweather-redis`
3. Region: Same as database
4. Plan: Free (or Starter $10/month)
5. Click "Create Redis"
6. Copy the "Internal Redis URL"

### Step 4: Create Web Service

1. Click "New +" → "Web Service"
2. Connect your GitHub repository
3. Configure:
   - **Name:** `afweather-backend`
   - **Region:** Same as database
   - **Branch:** `main`
   - **Root Directory:** `backend` (if in subdirectory)
   - **Runtime:** Node
   - **Build Command:** `npm install && npm run build`
   - **Start Command:** `npm start`
   - **Plan:** Free (or Starter $7/month)

### Step 5: Add Environment Variables

In the "Environment" tab, add:

```
NODE_ENV=production
PORT=3000

# Database (paste Internal Database URL from Step 2)
DATABASE_URL=postgresql://...

# Redis (paste Internal Redis URL from Step 3)
REDIS_URL=redis://...

# JWT Secrets
JWT_SECRET=your_super_secure_jwt_secret
JWT_REFRESH_SECRET=your_super_secure_refresh_secret

# Weather APIs
OPENWEATHER_API_KEY=your_key
WEATHERAPI_KEY=your_key
VISUALCROSSING_API_KEY=your_key

# Payment Providers
STRIPE_SECRET_KEY=sk_live_...
MPESA_CONSUMER_KEY=...
PAYSTACK_SECRET_KEY=sk_live_...

# CORS
CORS_ORIGIN=*
```

### Step 6: Deploy

1. Click "Create Web Service"
2. Render will build and deploy (3-5 minutes)
3. Your API will be available at: `https://afweather-backend.onrender.com`

### Step 7: Test

```bash
curl https://afweather-backend.onrender.com/health
```

---

## 🎯 Deployment Option 3: DigitalOcean (VPS)

**Best for:** Full control, custom configuration  
**Cost:** $5-50/month  
**Time:** 1-2 hours

### Step 1: Create Droplet

1. Go to https://digitalocean.com
2. Create account
3. Click "Create" → "Droplets"
4. Choose:
   - **Image:** Ubuntu 22.04 LTS
   - **Plan:** Basic ($5/month or higher)
   - **Region:** Closest to users
5. Add SSH key
6. Click "Create Droplet"

### Step 2: Connect to Server

```bash
ssh root@your_droplet_ip
```

### Step 3: Install Dependencies

```bash
# Update system
apt update && apt upgrade -y

# Install Node.js 18
curl -fsSL https://deb.nodesource.com/setup_18.x | bash -
apt install -y nodejs

# Install PostgreSQL
apt install -y postgresql postgresql-contrib

# Install Redis
apt install -y redis-server

# Install Nginx
apt install -y nginx

# Install PM2 (process manager)
npm install -g pm2
```

### Step 4: Set Up Database

```bash
# Switch to postgres user
sudo -u postgres psql

# Create database and user
CREATE DATABASE afweather;
CREATE USER afweather_user WITH PASSWORD 'secure_password';
GRANT ALL PRIVILEGES ON DATABASE afweather TO afweather_user;
\q
```

### Step 5: Deploy Application

```bash
# Create app directory
mkdir -p /var/www/afweather-backend
cd /var/www/afweather-backend

# Clone your repository
git clone https://github.com/YOUR_USERNAME/afweather-backend.git .

# Install dependencies
npm install

# Create .env file
nano .env
```

Paste your environment variables, then:

```bash
# Build TypeScript
npm run build

# Start with PM2
pm2 start dist/server.js --name afweather-backend
pm2 save
pm2 startup
```

### Step 6: Configure Nginx

```bash
nano /etc/nginx/sites-available/afweather
```

Paste:
```nginx
server {
    listen 80;
    server_name your_domain.com;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

Enable site:
```bash
ln -s /etc/nginx/sites-available/afweather /etc/nginx/sites-enabled/
nginx -t
systemctl restart nginx
```

### Step 7: Set Up SSL (Optional but Recommended)

```bash
apt install -y certbot python3-certbot-nginx
certbot --nginx -d your_domain.com
```

---

## 🎯 Deployment Option 4: Docker (Any Platform)

**Best for:** Consistent deployment across platforms  
**Works on:** AWS, GCP, Azure, any VPS

### Step 1: Build Docker Image

```bash
cd C:\Users\Admin\CascadeProjects\WeatherAfrica\backend
docker build -t afweather-backend .
```

### Step 2: Push to Registry

```bash
# Tag image
docker tag afweather-backend your-registry/afweather-backend:latest

# Push to Docker Hub
docker push your-registry/afweather-backend:latest
```

### Step 3: Deploy with Docker Compose

Create `docker-compose.prod.yml`:
```yaml
version: '3.8'

services:
  postgres:
    image: postgres:16-alpine
    environment:
      POSTGRES_DB: afweather
      POSTGRES_USER: afweather_user
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    volumes:
      - postgres_data:/var/lib/postgresql/data

  redis:
    image: redis:7-alpine
    volumes:
      - redis_data:/data

  api:
    image: your-registry/afweather-backend:latest
    depends_on:
      - postgres
      - redis
    ports:
      - "3000:3000"
    environment:
      NODE_ENV: production
      DB_HOST: postgres
      REDIS_HOST: redis
      # Add all other env vars
    restart: unless-stopped

volumes:
  postgres_data:
  redis_data:
```

Deploy:
```bash
docker-compose -f docker-compose.prod.yml up -d
```

---

## 🔄 Post-Deployment Tasks

### 1. Test All Endpoints

```bash
# Health check
curl https://your-api-url.com/health

# Register user
curl -X POST https://your-api-url.com/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com","password":"password123","name":"Test User"}'

# Get weather
curl https://your-api-url.com/api/weather/current?lat=-1.286389&lon=36.817223 \
  -H "Authorization: Bearer YOUR_TOKEN"
```

### 2. Set Up Monitoring

**Option A: Railway/Render Built-in**
- Check metrics in dashboard
- Set up alerts

**Option B: External Monitoring**
- UptimeRobot (free)
- Datadog
- New Relic

### 3. Set Up Backups

**Database Backups:**
```bash
# Daily backup script
pg_dump -U afweather_user afweather > backup_$(date +%Y%m%d).sql
```

**Automated Backups:**
- Railway: Automatic
- Render: Automatic on paid plans
- DigitalOcean: Set up via dashboard

### 4. Configure Custom Domain (Optional)

1. Buy domain (Namecheap, GoDaddy, etc.)
2. Add DNS records:
   - Type: A
   - Name: @
   - Value: Your server IP
3. Wait for DNS propagation (up to 48 hours)

### 5. Update React Native App

In `WeatherAfrica/.env`:
```
BACKEND_API_URL=https://your-deployed-api-url.com
```

Rebuild app:
```bash
cd C:\Users\Admin\CascadeProjects\WeatherAfrica
npm run android
```

---

## 📊 Monitoring & Maintenance

### Check Logs

**Railway/Render:**
- View in dashboard

**DigitalOcean/VPS:**
```bash
pm2 logs afweather-backend
tail -f logs/combined.log
```

### Update Application

**Railway/Render:**
- Push to GitHub, auto-deploys

**DigitalOcean:**
```bash
cd /var/www/afweather-backend
git pull
npm install
npm run build
pm2 restart afweather-backend
```

### Scale Up

**Railway/Render:**
- Upgrade plan in dashboard

**DigitalOcean:**
- Resize droplet
- Add load balancer
- Deploy multiple instances

---

## 🆘 Troubleshooting

### API Not Responding
1. Check server status
2. Check logs
3. Verify environment variables
4. Check database connection

### Database Connection Failed
1. Verify DATABASE_URL
2. Check PostgreSQL is running
3. Check firewall rules
4. Verify credentials

### High Response Times
1. Check Redis is working
2. Add database indexes
3. Enable caching
4. Scale up server

---

## ✅ Deployment Complete!

Your AfWeather backend is now live and ready to serve billions of farmers!

**Next Steps:**
1. Test all features thoroughly
2. Monitor performance
3. Set up alerts
4. Plan scaling strategy
5. Deploy React Native app

**Your API URL:** `https://your-deployed-url.com`

**Health Check:** `https://your-deployed-url.com/health`

---

**Need Help?** Contact support@afweather.app
