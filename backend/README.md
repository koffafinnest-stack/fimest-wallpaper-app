# Fimest Wallpaper - Backend API

Node.js/Express REST API for the Fimest Wallpaper application.

## Prerequisites

- Node.js 16+
- npm or yarn
- PostgreSQL 12+

## Installation

1. **Install dependencies**
   ```bash
   npm install
   ```

2. **Set up environment variables**
   ```bash
   cp .env.example .env
   ```
   Edit `.env` with your database credentials and other settings.

3. **Set up database**
   ```bash
   # Create database
   createdb fimest_wallpaper_db
   
   # Run schema
   psql -U postgres -d fimest_wallpaper_db -f database.sql
   ```

## Running the Server

**Development mode (with auto-reload)**
```bash
npm run dev
```

**Production mode**
```bash
npm start
```

The API will be available at `http://localhost:5000`

## API Endpoints

### Authentication
- `POST /api/auth/register` - Register new user
- `POST /api/auth/login` - Login user

### Wallpapers
- `GET /api/wallpapers` - Get all wallpapers (with pagination)
- `GET /api/wallpapers/:id` - Get single wallpaper
- `POST /api/wallpapers` - Create wallpaper (requires auth)

### Users
- `GET /api/users/profile` - Get user profile (requires auth)
- `PUT /api/users/profile` - Update user profile (requires auth)

### Favorites
- `GET /api/favorites` - Get user favorites (requires auth)
- `POST /api/favorites/:wallpaperId` - Add to favorites (requires auth)
- `DELETE /api/favorites/:wallpaperId` - Remove from favorites (requires auth)

## Database Schema

See `database.sql` for the complete schema with tables:
- `users` - User accounts
- `wallpapers` - Wallpaper content
- `favorites` - User favorite wallpapers

## Environment Variables

```
DB_HOST=localhost
DB_PORT=5432
DB_NAME=fimest_wallpaper_db
DB_USER=postgres
DB_PASSWORD=your_password
PORT=5000
NODE_ENV=development
JWT_SECRET=your_secret_key
JWT_EXPIRE=7d
CORS_ORIGIN=http://localhost:3000
```

## Technologies Used

- Express.js - Web framework
- PostgreSQL - Database
- JWT - Authentication
- bcryptjs - Password hashing
- CORS - Cross-origin resource sharing

## License

MIT
