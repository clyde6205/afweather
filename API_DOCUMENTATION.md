# 📖 AfWeather Backend - Complete API Documentation

**Base URL:** `https://your-backend-url.com`  
**Version:** v1  
**Authentication:** JWT Bearer Token

---

## 📋 Table of Contents

1. [Authentication](#authentication)
2. [Weather](#weather)
3. [Community](#community)
4. [Profile](#profile)
5. [Contacts](#contacts)
6. [Payments](#payments)
7. [Analytics](#analytics)
8. [Error Handling](#error-handling)

---

## 🔐 Authentication

All protected endpoints require an `Authorization` header:
```
Authorization: Bearer <your_jwt_token>
```

### Register User

**POST** `/api/auth/register`

**Request Body:**
```json
{
  "email": "farmer@example.com",
  "password": "securepassword123",
  "name": "John Farmer",
  "phone": "+254712345678"
}
```

**Response:** `201 Created`
```json
{
  "success": true,
  "data": {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "user": {
      "id": "uuid",
      "email": "farmer@example.com",
      "name": "John Farmer",
      "phone": "+254712345678",
      "subscription_tier": "free",
      "created_at": "2025-10-14T22:00:00.000Z"
    }
  }
}
```

---

### Login

**POST** `/api/auth/login`

**Request Body:**
```json
{
  "email": "farmer@example.com",
  "password": "securepassword123"
}
```

**Response:** `200 OK`
```json
{
  "success": true,
  "data": {
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "refreshToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
    "user": {
      "id": "uuid",
      "email": "farmer@example.com",
      "name": "John Farmer",
      "subscription_tier": "free"
    }
  }
}
```

---

### Refresh Token

**POST** `/api/auth/refresh-token`

**Request Body:**
```json
{
  "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
}
```

**Response:** `200 OK`
```json
{
  "success": true,
  "data": {
    "token": "new_access_token",
    "refreshToken": "new_refresh_token"
  }
}
```

---

### Get Current User

**GET** `/api/auth/me`

**Headers:** `Authorization: Bearer <token>`

**Response:** `200 OK`
```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "email": "farmer@example.com",
    "name": "John Farmer",
    "phone": "+254712345678",
    "profile_image_url": "https://...",
    "subscription_tier": "free",
    "created_at": "2025-10-14T22:00:00.000Z"
  }
}
```

---

### Forgot Password

**POST** `/api/auth/forgot-password`

**Request Body:**
```json
{
  "email": "farmer@example.com"
}
```

**Response:** `200 OK`
```json
{
  "success": true,
  "message": "Password reset instructions sent to email"
}
```

---

## 🌤️ Weather

### Get Current Weather

**GET** `/api/weather/current?lat={latitude}&lon={longitude}`

**Headers:** `Authorization: Bearer <token>`

**Query Parameters:**
- `lat` (required): Latitude (-90 to 90)
- `lon` (required): Longitude (-180 to 180)

**Example:**
```
GET /api/weather/current?lat=-1.286389&lon=36.817223
```

**Response:** `200 OK`
```json
{
  "success": true,
  "data": {
    "temperature": 22.5,
    "feels_like": 21.8,
    "humidity": 65,
    "pressure": 1013,
    "wind_speed": 3.5,
    "wind_direction": 180,
    "description": "partly cloudy",
    "icon": "02d",
    "timestamp": "2025-10-14T22:00:00.000Z"
  }
}
```

---

### Get Weather Forecast

**GET** `/api/weather/forecast?lat={latitude}&lon={longitude}`

**Headers:** `Authorization: Bearer <token>`

**Response:** `200 OK`
```json
{
  "success": true,
  "data": [
    {
      "date": "2025-10-15",
      "temp_max": 28.5,
      "temp_min": 18.2,
      "humidity": 60,
      "precipitation": 0.5,
      "wind_speed": 4.2,
      "description": "light rain",
      "icon": "10d"
    },
    // ... 6 more days
  ]
}
```

---

## 👥 Community

### Get All Posts

**GET** `/api/community/posts?page={page}&limit={limit}`

**Headers:** `Authorization: Bearer <token>`

**Query Parameters:**
- `page` (optional): Page number (default: 1)
- `limit` (optional): Posts per page (default: 20)

**Response:** `200 OK`
```json
{
  "success": true,
  "data": [
    {
      "id": "uuid",
      "content": "Great harvest this season!",
      "image_url": "https://...",
      "likes": 15,
      "created_at": "2025-10-14T20:00:00.000Z",
      "user": {
        "id": "uuid",
        "name": "John Farmer",
        "profile_image_url": "https://..."
      }
    }
  ],
  "total": 150,
  "page": 1,
  "limit": 20,
  "totalPages": 8
}
```

---

### Create Post

**POST** `/api/community/posts`

**Headers:** `Authorization: Bearer <token>`

**Request Body:**
```json
{
  "content": "Just planted maize on 5 acres!",
  "image_url": "https://..."
}
```

**Response:** `201 Created`
```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "content": "Just planted maize on 5 acres!",
    "image_url": "https://...",
    "likes": 0,
    "created_at": "2025-10-14T22:00:00.000Z",
    "user": {
      "id": "uuid",
      "name": "John Farmer",
      "profile_image_url": "https://..."
    }
  }
}
```

---

### Like/Unlike Post

**POST** `/api/community/posts/:id/like`

**Headers:** `Authorization: Bearer <token>`

**Response:** `200 OK`
```json
{
  "success": true,
  "data": {
    "liked": true,
    "likes": 16
  }
}
```

---

### Delete Post

**DELETE** `/api/community/posts/:id`

**Headers:** `Authorization: Bearer <token>`

**Response:** `200 OK`
```json
{
  "success": true,
  "message": "Post deleted successfully"
}
```

---

## 👤 Profile

### Get Profile

**GET** `/api/profile`

**Headers:** `Authorization: Bearer <token>`

**Response:** `200 OK`
```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "email": "farmer@example.com",
    "name": "John Farmer",
    "phone": "+254712345678",
    "profile_image_url": "https://...",
    "location_lat": -1.286389,
    "location_lon": 36.817223,
    "subscription_tier": "premium",
    "created_at": "2025-10-14T22:00:00.000Z"
  }
}
```

---

### Update Profile

**PUT** `/api/profile`

**Headers:** `Authorization: Bearer <token>`

**Request Body:**
```json
{
  "name": "John Updated Farmer",
  "phone": "+254712345679",
  "location_lat": -1.286389,
  "location_lon": 36.817223
}
```

**Response:** `200 OK`
```json
{
  "success": true,
  "data": {
    // Updated profile data
  }
}
```

---

### Upload Profile Image

**POST** `/api/profile/image`

**Headers:** 
- `Authorization: Bearer <token>`
- `Content-Type: multipart/form-data`

**Form Data:**
- `image`: File (JPEG, PNG, GIF, max 5MB)

**Response:** `200 OK`
```json
{
  "success": true,
  "data": {
    "profile_image_url": "https://your-backend-url.com/uploads/profiles/profile-123456.jpg"
  }
}
```

---

## 📞 Contacts

### Get All Contacts

**GET** `/api/contacts`

**Headers:** `Authorization: Bearer <token>`

**Response:** `200 OK`
```json
{
  "success": true,
  "contacts": [
    {
      "id": "uuid",
      "name": "Jane Doe",
      "phone": "+254712345678",
      "created_at": "2025-10-14T22:00:00.000Z"
    }
  ]
}
```

---

### Add Contact

**POST** `/api/contacts`

**Headers:** `Authorization: Bearer <token>`

**Request Body:**
```json
{
  "name": "Jane Doe",
  "phone": "+254712345678"
}
```

**Response:** `201 Created`
```json
{
  "success": true,
  "data": {
    "id": "uuid",
    "name": "Jane Doe",
    "phone": "+254712345678",
    "created_at": "2025-10-14T22:00:00.000Z"
  }
}
```

---

### Update Contact

**PUT** `/api/contacts/:id`

**Headers:** `Authorization: Bearer <token>`

**Request Body:**
```json
{
  "name": "Jane Updated",
  "phone": "+254712345679"
}
```

---

### Delete Contact

**DELETE** `/api/contacts/:id`

**Headers:** `Authorization: Bearer <token>`

**Response:** `200 OK`
```json
{
  "success": true,
  "message": "Contact deleted successfully"
}
```

---

## 💳 Payments

### Create Stripe Subscription

**POST** `/api/payments/stripe/subscribe`

**Headers:** `Authorization: Bearer <token>`

**Request Body:**
```json
{
  "tierId": "premium",
  "paymentMethodId": "pm_card_visa"
}
```

**Response:** `200 OK`
```json
{
  "success": true,
  "data": {
    "subscription_id": "sub_123456",
    "status": "active",
    "tier": "premium"
  }
}
```

---

### Create M-Pesa Subscription

**POST** `/api/payments/mpesa/subscribe`

**Headers:** `Authorization: Bearer <token>`

**Request Body:**
```json
{
  "tierId": "premium",
  "phoneNumber": "254712345678"
}
```

**Response:** `200 OK`
```json
{
  "success": true,
  "data": {
    "transaction_id": "ws_CO_123456",
    "status": "pending",
    "message": "Check your phone for M-Pesa prompt"
  }
}
```

---

### Get Payment Status

**GET** `/api/payments/status/:transactionId`

**Headers:** `Authorization: Bearer <token>`

**Response:** `200 OK`
```json
{
  "success": true,
  "data": {
    "transaction_id": "ws_CO_123456",
    "status": "completed",
    "amount": 649,
    "currency": "KES",
    "provider": "mpesa",
    "created_at": "2025-10-14T22:00:00.000Z"
  }
}
```

---

### Cancel Subscription

**POST** `/api/payments/cancel`

**Headers:** `Authorization: Bearer <token>`

**Response:** `200 OK`
```json
{
  "success": true,
  "message": "Subscription cancelled successfully"
}
```

---

## 📊 Analytics

### Track Phone Click

**POST** `/api/analytics/phone-click`

**Request Body:**
```json
{
  "phoneId": "phone_123"
}
```

**Response:** `200 OK`
```json
{
  "success": true,
  "message": "Event tracked"
}
```

---

### Get Dashboard Stats (Admin Only)

**GET** `/api/analytics/dashboard`

**Headers:** `Authorization: Bearer <admin_token>`

**Response:** `200 OK`
```json
{
  "success": true,
  "data": {
    "commissions": {
      "total_clicks": 1250,
      "total_conversions": 45,
      "estimated_earnings": 225
    },
    "ad_revenue": {
      "total_revenue": 150.50,
      "by_ad_type": {
        "banner": 50.25,
        "interstitial": 100.25
      }
    },
    "subscription_revenue": {
      "total_revenue": 2500,
      "by_tier": {
        "premium": 1500,
        "upgrade": 1000
      },
      "total_subscriptions": 350
    },
    "total_revenue": 2875.50
  }
}
```

---

## ❌ Error Handling

All errors follow this format:

**Response:** `4xx` or `5xx`
```json
{
  "success": false,
  "error": "Error message here"
}
```

### Common Error Codes

- **400 Bad Request:** Invalid input data
- **401 Unauthorized:** Missing or invalid token
- **403 Forbidden:** Insufficient permissions
- **404 Not Found:** Resource not found
- **429 Too Many Requests:** Rate limit exceeded
- **500 Internal Server Error:** Server error

### Example Errors

**Invalid Credentials:**
```json
{
  "success": false,
  "error": "Invalid credentials"
}
```

**Token Expired:**
```json
{
  "success": false,
  "error": "Invalid or expired token"
}
```

**Validation Error:**
```json
{
  "success": false,
  "error": "Validation error",
  "details": [
    {
      "path": ["body", "email"],
      "message": "Invalid email"
    }
  ]
}
```

---

## 🔒 Rate Limits

- **General API:** 100 requests per 15 minutes
- **Authentication:** 5 attempts per 15 minutes
- **Weather API:** 30 requests per minute

When rate limit is exceeded:
```json
{
  "success": false,
  "error": "Too many requests, please try again later"
}
```

---

## 📝 Notes

- All timestamps are in ISO 8601 format (UTC)
- All coordinates use decimal degrees
- File uploads limited to 5MB
- Weather data cached for 10 minutes (current) and 1 hour (forecast)
- Subscription tiers: `free`, `premium`, `upgrade`

---

**API Version:** v1  
**Last Updated:** October 2025  
**Support:** support@afweather.app
