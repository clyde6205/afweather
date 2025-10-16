# ⚡ AfWeather Backend - Quick Start Guide

**Get your backend running in 10 minutes!**

---

## 🎯 What You're Building

An **enterprise-grade backend** that serves:
- ✅ Billions of farmers globally
- ✅ Real-time weather data (3 providers)
- ✅ Offline-first sync support
- ✅ Community features
- ✅ Payment processing (Stripe, M-Pesa, Paystack)
- ✅ Analytics & tracking

---

## 🚀 Quick Start (Local Development)

### Step 1: Install Dependencies (2 minutes)

```bash
cd C:\Users\Admin\CascadeProjects\WeatherAfrica\backend
npm install
```

### Step 2: Start Database & Redis (1 minute)

**Option A: Using Docker (Recommended)**
```bash
npm run docker:dev
```

This starts PostgreSQL and Redis automatically.

**Option B: Manual Installation**
- Install PostgreSQL 14+ from https://www.postgresql.org/download/windows/
- Install Redis from https://github.com/microsoftarchive/redis/releases

### Step 3: Configure Environment (2 minutes)

```bash
# Copy example env file
copy .env.example .env

# Edit .env in notepad
notepad .env
```

**Minimum required configuration:**
```
NODE_ENV=development
PORT=3000

# Database (if using Docker, these are correct)
DB_HOST=localhost
DB_PORT=5432
DB_NAME=afweather
DB_USER=postgres
DB_PASSWORD=postgres

# Redis (if using Docker, these are correct)
REDIS_HOST=localhost
REDIS_PORT=6379

# JWT Secrets (CHANGE THESE!)
JWT_SECRET=your_super_secure_jwt_secret_change_this
JWT_REFRESH_SECRET=your_super_secure_refresh_secret_change_this

# Weather API (get free key from openweathermap.org)
OPENWEATHER_API_KEY=your_openweathermap_api_key
DEFAULT_WEATHER_PROVIDER=openweathermap
```

### Step 4: Start Development Server (1 minute)

```bash
npm run dev
```

You should see:
```
✅ Database connection established
✅ Redis connected
✅ Server started successfully!
🌍 Environment: development
🔗 Server running on port 3000
📡 API available at: http://localhost:3000/api
💚 Health check: http://localhost:3000/health
```

### Step 5: Test Your API (2 minutes)

**Test 1: Health Check**
```bash
curl http://localhost:3000/health
```

**Test 2: Register a User**
```bash
curl -X POST http://localhost:3000/api/auth/register ^
  -H "Content-Type: application/json" ^
  -d "{\"email\":\"farmer@example.com\",\"password\":\"password123\",\"name\":\"John Farmer\"}"
```

**Test 3: Login**
```bash
curl -X POST http://localhost:3000/api/auth/login ^
  -H "Content-Type: application/json" ^
  -d "{\"email\":\"farmer@example.com\",\"password\":\"password123\"}"
```

Copy the `token` from the response.

**Test 4: Get Weather**
```bash
curl "http://localhost:3000/api/weather/current?lat=-1.286389&lon=36.817223" ^
  -H "Authorization: Bearer YOUR_TOKEN_HERE"
```

---

## ✅ Success! Your Backend is Running!

**What you have now:**
- ✅ Backend API running on `http://localhost:3000`
- ✅ PostgreSQL database with all tables
- ✅ Redis caching enabled
- ✅ All 25+ API endpoints ready
- ✅ Authentication working
- ✅ Weather data flowing

---

## 🔗 Connect to React Native Frontend

### Step 1: Update Frontend .env

```bash
cd C:\Users\Admin\CascadeProjects\WeatherAfrica
notepad .env
```

Add:
```
BACKEND_API_URL=http://10.0.2.2:3000
```

**Note:** Use `10.0.2.2` for Android emulator (maps to localhost)

### Step 2: Start React Native App

```bash
npm start
# In another terminal:
npm run android
```

### Step 3: Test in App

1. Open app on emulator/device
2. Register a new account
3. Login
4. Check weather for your location
5. Create a community post
6. Upload profile picture

**Everything should work!**

---

## 🚀 Deploy to Production

When ready to deploy:

1. **Choose a platform:**
   - Railway.app (easiest, $5-20/month)
   - Render.com (free tier available)
   - DigitalOcean ($5-50/month)

2. **Follow deployment guide:**
   ```bash
   # Read the deployment guide
   notepad DEPLOYMENT.md
   ```

3. **Update frontend with production URL:**
   ```
   BACKEND_API_URL=https://your-deployed-backend.com
   ```

---

## 📚 Next Steps

### Learn More
- **Full Documentation:** `README.md`
- **Deployment Guide:** `DEPLOYMENT.md`
- **Connect Frontend:** `CONNECT_FRONTEND.md`
- **API Reference:** `API_DOCUMENTATION.md`

### Get Weather API Keys
- OpenWeatherMap: https://openweathermap.org/api (free tier: 1000 calls/day)
- WeatherAPI: https://www.weatherapi.com/ (free tier: 1M calls/month)
- Visual Crossing: https://www.visualcrossing.com/ (free tier: 1000 calls/day)

### Set Up Payments (Optional)
- Stripe: https://stripe.com (test mode free)
- M-Pesa: https://developer.safaricom.co.ke/
- Paystack: https://paystack.com/ (Africa)

---

## 🐛 Troubleshooting

### "Database connection failed"
- Ensure PostgreSQL is running
- Check credentials in `.env`
- If using Docker: `npm run docker:dev`

### "Redis Client Error"
- Ensure Redis is running
- If using Docker: `npm run docker:dev`

### "Port 3000 already in use"
- Change PORT in `.env` to 3001
- Or kill process using port 3000

### "Cannot find module"
- Run `npm install` again
- Delete `node_modules` and reinstall

---

## 💡 Development Tips

### Watch Mode
```bash
npm run dev
```
Auto-restarts on file changes.

### View Logs
```bash
# In development, logs show in console
# In production, check:
tail -f logs/combined.log
tail -f logs/error.log
```

### Database GUI
Use **pgAdmin** or **DBeaver** to view database:
- Host: localhost
- Port: 5432
- Database: afweather
- User: postgres
- Password: postgres

### Redis GUI
Use **RedisInsight** to view cache:
- Host: localhost
- Port: 6379

---

## ✅ You're Ready!

Your backend is running and ready to serve billions of farmers!

**Backend URL:** `http://localhost:3000`  
**Health Check:** `http://localhost:3000/health`  
**API Docs:** `http://localhost:3000/api`

**Questions?** Check `README.md` or `DEPLOYMENT.md`

---

**Built with ❤️ for farmers worldwide** 🌾
