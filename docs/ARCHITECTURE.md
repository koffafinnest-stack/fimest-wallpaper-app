# Fimest Wallpaper - Architecture & Design

## System Overview

Fimest Wallpaper is a modern, cross-platform wallpaper application with a client-server architecture.

```
┌─────────────────────────────────────────────────────────────┐
│                    Flutter Client Apps                       │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │   Android    │  │     iOS      │  │   Desktop    │      │
│  │   (Mobile)   │  │   (Mobile)   │  │  (Mac/Win)   │      │
│  └──────────────┘  └──────────────┘  └──────────────┘      │
└─────────────────────────────────────────────────────────────┘
                           ↓ (HTTP REST API)
┌─────────────────────────────────────────────────────────────┐
│                   Express.js Backend                         │
│  ┌──────────────────────────────────────────────────────┐   │
│  │          REST API (Node.js/Express)                  │   │
│  │  • Authentication & Authorization (JWT)              │   │
│  │  • Wallpaper Management                              │   │
│  │  • User Profile & Favorites                          │   │
│  └──────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────────┐
│                   PostgreSQL Database                        │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐      │
│  │   users      │  │ wallpapers   │  │ favorites    │      │
│  └──────────────┘  └──────────────┘  └──────────────┘      │
└─────────────────────────────────────────────────────────────┘
```

## Technology Stack

### Frontend (Flutter)
- **Language:** Dart
- **Framework:** Flutter 3.0+
- **State Management:** Provider
- **HTTP Client:** http package
- **Local Storage:** shared_preferences
- **UI Framework:** Material Design 3

**Advantages:**
- Single codebase for iOS, Android, macOS, Windows, Linux
- Fast compilation and hot reload
- Native performance
- Beautiful, modern UI out of the box

### Backend (Node.js)
- **Runtime:** Node.js 16+
- **Framework:** Express.js
- **Language:** JavaScript
- **Authentication:** JWT (jsonwebtoken)
- **Password Hashing:** bcryptjs
- **Database Driver:** pg (PostgreSQL)
- **Validation:** joi

**Advantages:**
- JavaScript everywhere (frontend & backend)
- Large ecosystem and community
- Fast API development
- Easy to learn and maintain

### Database (PostgreSQL)
- **Type:** Relational SQL Database
- **Version:** 12+
- **Features:** ACID compliance, JSON support, full-text search

**Schema:**
- `users` - User accounts and authentication
- `wallpapers` - Wallpaper content metadata
- `favorites` - User favorite relationships

## Data Flow

### 1. User Registration
```
Flutter App → /auth/register (POST) → Backend → Hash Password → Store in DB
                                         ↓
                                      Return JWT Token
                                         ↓
                                      Flutter App (Save Token Locally)
```

### 2. Browsing Wallpapers
```
Flutter App → /wallpapers (GET) → Backend → Query Database → Return Wallpaper List
                                              ↓
                                           Render Grid UI
```

### 3. Adding to Favorites
```
Flutter App → /favorites/:id (POST) → Backend (Verify Token) → Store Relationship
                    ↓
              Update UI (Heart Icon)
```

## API Communication

### Request/Response Format

**Request Headers:**
```
Content-Type: application/json
Authorization: Bearer <JWT_TOKEN>  (if protected endpoint)
```

**Response Headers:**
```
Content-Type: application/json
```

### Error Handling
- 400: Bad Request (invalid input)
- 401: Unauthorized (missing/invalid token)
- 404: Not Found (resource doesn't exist)
- 500: Internal Server Error

## Security

### Authentication
- JWT tokens for stateless authentication
- Token stored securely in device's local storage
- Tokens include user ID and email
- Expiration time configurable (default: 7 days)

### Password Security
- bcryptjs for password hashing
- Salt rounds: 10
- Never store plain text passwords

### CORS
- Configured to allow specific origins
- Prevents unauthorized cross-origin requests

## Database Schema

### users table
```sql
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  name VARCHAR(255) NOT NULL,
  email VARCHAR(255) UNIQUE NOT NULL,
  password VARCHAR(255) NOT NULL,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### wallpapers table
```sql
CREATE TABLE wallpapers (
  id SERIAL PRIMARY KEY,
  title VARCHAR(255) NOT NULL,
  description TEXT,
  category VARCHAR(100) NOT NULL,
  image_url VARCHAR(500),
  video_url VARCHAR(500),
  duration INTEGER,
  active BOOLEAN DEFAULT true,
  created_by INTEGER REFERENCES users(id),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### favorites table
```sql
CREATE TABLE favorites (
  id SERIAL PRIMARY KEY,
  user_id INTEGER NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  wallpaper_id INTEGER NOT NULL REFERENCES wallpapers(id) ON DELETE CASCADE,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
  UNIQUE(user_id, wallpaper_id)
);
```

## Deployment Architecture

### Development
```
Local Machine
├── Backend (localhost:5000)
└── Mobile/Desktop App (connected to local API)
```

### Production (Recommended)
```
Cloud Platform (AWS, Heroku, DigitalOcean, etc.)
├── Backend API (Deployed Node.js)
├── PostgreSQL Database
└── CDN (for image/video content)

Mobile Apps
└── Connect to cloud API endpoint
```

## Scalability Considerations

### Current Limitations
- Single backend instance
- No caching layer
- No CDN for media

### Future Improvements
1. **Load Balancing**
   - Multiple backend instances
   - Nginx/HAProxy reverse proxy

2. **Caching**
   - Redis for session/data caching
   - CloudFlare for CDN

3. **Database Optimization**
   - Connection pooling
   - Query indexing
   - Read replicas

4. **Media Hosting**
   - AWS S3 for image/video storage
   - CloudFront CDN
   - Lazy loading & compression

5. **Monitoring**
   - ELK Stack (logs)
   - Prometheus (metrics)
   - Sentry (error tracking)

## Performance Metrics

### Target KPIs
- API response time: < 200ms
- App startup time: < 3s
- Image load time: < 500ms
- 99.9% uptime

### Optimization Strategies
1. Pagination (20 items per page)
2. Image compression
3. Lazy loading
4. Code splitting
5. Database indexing

## Development Workflow

### Setup
```bash
# Backend
cd backend
npm install
createdb fimest_wallpaper_db
psql -d fimest_wallpaper_db -f database.sql
npm run dev

# Mobile App
cd mobile_app
flutter pub get
flutter run
```

### Testing
```bash
# Backend unit tests
npm test

# Flutter widget tests
flutter test
```

### Deployment
```bash
# Build APK (Android)
flutter build apk --release

# Build IPA (iOS)
flutter build ios --release

# Deploy Backend
npm run build
npm start
```

## Security Best Practices

- ✅ Hash passwords with bcryptjs
- ✅ Use JWT for stateless auth
- ✅ Validate all inputs on backend
- ✅ Use HTTPS in production
- ✅ Enable CORS properly
- ✅ Rate limit API endpoints
- ✅ Keep dependencies updated
- ✅ Use environment variables for secrets

## Monitoring & Logging

### Recommended Tools
- **Logging:** Winston or Pino
- **Error Tracking:** Sentry
- **Performance:** New Relic or Datadog
- **Uptime:** Pingdom or UptimeRobot

---

**Architecture Version:** 1.0  
**Last Updated:** 2024-01-15
