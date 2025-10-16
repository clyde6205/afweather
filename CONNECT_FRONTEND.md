# 🔗 Connecting React Native Frontend to Backend

## 📍 Step-by-Step Connection Guide

### Step 1: Deploy Your Backend

First, deploy your backend using one of the methods in `DEPLOYMENT.md`. You'll get a URL like:
- Railway: `https://afweather-backend.up.railway.app`
- Render: `https://afweather-backend.onrender.com`
- Custom: `https://api.afweather.app`

### Step 2: Update Frontend Environment

1. **Navigate to frontend directory:**
```bash
cd C:\Users\Admin\CascadeProjects\WeatherAfrica
```

2. **Create or edit `.env` file:**
```bash
# Open in notepad
notepad .env
```

3. **Add your backend URL:**
```
# Backend API - PASTE YOUR DEPLOYED URL HERE
BACKEND_API_URL=https://your-deployed-backend-url.com

# Weather API (optional - backend handles this)
WEATHER_API_KEY=your_openweathermap_key
WEATHER_API_URL=https://api.openweathermap.org/data/2.5
WEATHER_PROVIDER=openweathermap

# WhatsApp (Optional)
WHATSAPP_BUSINESS_NUMBER=+1234567890
```

4. **Save the file**

### Step 3: Verify Frontend Configuration

Your frontend already has the API service configured at:
`src/services/apiService.ts`

It reads from `process.env.BACKEND_API_URL` automatically.

### Step 4: Test Connection Locally

1. **Start Metro bundler:**
```bash
npm start
```

2. **In another terminal, run Android:**
```bash
npm run android
```

3. **Test the app:**
   - Register a new user
   - Login
   - Check weather
   - Create a community post
   - Upload profile picture

### Step 5: Verify API Calls

Check if API calls are working:

1. **Open React Native Debugger or Chrome DevTools**
2. **Watch network requests**
3. **Should see requests to your backend URL**

Example successful request:
```
POST https://your-backend-url.com/api/auth/login
Status: 200 OK
Response: {
  "success": true,
  "data": {
    "token": "eyJhbGc...",
    "user": {...}
  }
}
```

---

## 🧪 Testing Checklist

Test each feature to ensure backend connection works:

### Authentication
- [ ] Register new user
- [ ] Login with credentials
- [ ] Get current user profile
- [ ] Logout

### Weather
- [ ] Get current weather for location
- [ ] Get 7-day forecast
- [ ] Weather data displays correctly

### Community
- [ ] View community posts
- [ ] Create new post
- [ ] Like/unlike posts
- [ ] Delete own posts

### Profile
- [ ] View profile
- [ ] Update profile information
- [ ] Upload profile picture
- [ ] Picture displays correctly

### Contacts
- [ ] Add new contact
- [ ] View contacts list
- [ ] Update contact
- [ ] Delete contact

---

## 🐛 Troubleshooting

### Issue: "Network request failed"

**Cause:** Backend URL not configured or unreachable

**Fix:**
1. Verify `.env` file has correct URL
2. Check backend is running: `curl https://your-backend-url.com/health`
3. Rebuild app: `npm run android`

### Issue: "401 Unauthorized"

**Cause:** Token expired or invalid

**Fix:**
1. Logout and login again
2. Check token is being sent in headers
3. Verify JWT_SECRET matches between frontend and backend

### Issue: "CORS error"

**Cause:** Backend CORS not configured for your domain

**Fix:**
1. Update backend `.env`:
   ```
   CORS_ORIGIN=*
   ```
2. Or specify your domain:
   ```
   CORS_ORIGIN=https://your-frontend-domain.com
   ```
3. Redeploy backend

### Issue: "Timeout"

**Cause:** Backend taking too long to respond

**Fix:**
1. Check backend logs for errors
2. Verify database connection
3. Check Redis is running
4. Scale up backend server

### Issue: "Cannot connect to localhost"

**Cause:** Using `localhost` or `127.0.0.1` on Android emulator

**Fix:**
1. Use `10.0.2.2` for Android emulator
2. Or use your computer's IP address
3. Or deploy backend and use public URL

---

## 📱 Android Emulator Special Cases

If testing on Android emulator:

### Option 1: Use 10.0.2.2
```
BACKEND_API_URL=http://10.0.2.2:3000
```

### Option 2: Use Computer's IP
1. Find your IP:
   ```bash
   ipconfig
   ```
2. Look for "IPv4 Address" (e.g., 192.168.1.100)
3. Update `.env`:
   ```
   BACKEND_API_URL=http://192.168.1.100:3000
   ```

### Option 3: Use Deployed Backend (Recommended)
```
BACKEND_API_URL=https://your-deployed-backend.com
```

---

## 🚀 Production Deployment

### Step 1: Deploy Backend
Follow `DEPLOYMENT.md` to deploy to Railway, Render, or DigitalOcean.

### Step 2: Update Frontend .env
```
BACKEND_API_URL=https://your-production-backend-url.com
```

### Step 3: Build Release APK
```bash
cd android
./gradlew assembleRelease
```

APK will be at:
`android/app/build/outputs/apk/release/app-release.apk`

### Step 4: Test Release Build
1. Install APK on device
2. Test all features
3. Verify API calls work

### Step 5: Deploy to Play Store
Follow `PLAY_STORE_DEPLOYMENT.md` in your frontend docs.

---

## 🔒 Security Checklist

Before going to production:

- [ ] Backend uses HTTPS (not HTTP)
- [ ] JWT secrets are strong and unique
- [ ] CORS configured properly
- [ ] Rate limiting enabled
- [ ] API keys not exposed in frontend
- [ ] Environment variables not committed to Git
- [ ] Database credentials secure
- [ ] Payment provider keys are LIVE keys (not test)

---

## 📊 Monitoring Connection Health

### Backend Health Check
```bash
curl https://your-backend-url.com/health
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

### Test Authentication
```bash
curl -X POST https://your-backend-url.com/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"test@example.com","password":"password123"}'
```

### Test Weather API
```bash
curl "https://your-backend-url.com/api/weather/current?lat=-1.286389&lon=36.817223" \
  -H "Authorization: Bearer YOUR_TOKEN"
```

---

## ✅ Connection Complete!

Your React Native frontend is now connected to your backend!

**What You Have:**
- ✅ Full authentication system
- ✅ Real-time weather data
- ✅ Community features
- ✅ User profiles
- ✅ Payment processing
- ✅ Analytics tracking
- ✅ Offline-first sync

**Next Steps:**
1. Test thoroughly on real devices
2. Optimize performance
3. Add error handling
4. Set up crash reporting
5. Deploy to Play Store

---

**Need Help?** Check logs:
- **Frontend:** React Native Debugger
- **Backend:** `https://your-backend-url.com/logs` (if enabled)

**Support:** support@afweather.app
