# Quick Start Guide

## 🚀 Get Started in 5 Minutes

### Option 1: Docker (Easiest)

```bash
# Clone repo
git clone https://github.com/koffafinnest-stack/fimest-wallpaper-app.git
cd fimest-wallpaper-app

# Start everything with Docker
docker-compose up -d

# The app will be ready at http://localhost:5000
```

### Option 2: Manual Setup

#### 1. Setup Backend
```bash
cd backend
npm install
cp .env.example .env
# Edit .env with your PostgreSQL credentials

# Create database
createdb fimest_wallpaper_db
psql -d fimest_wallpaper_db -f database.sql

# Start server
npm run dev
```

#### 2. Setup Mobile App (in new terminal)
```bash
cd mobile_app
flutter pub get
flutter run
```

## 📱 Using the App

1. **Browse Wallpapers** - Home tab shows all available wallpapers
2. **Filter by Category** - Switch between Anime and Donghua
3. **View Details** - Tap any wallpaper to see full details
4. **Add to Favorites** - Heart icon to save your favorites
5. **Manage Profile** - Login/Register in Profile tab

## 🔐 Default Test Credentials

After setup, you can test with:
- **Email:** test@example.com
- **Password:** password123

## 📚 Full Documentation

- [Setup Guide](docs/SETUP.md) - Detailed installation
- [API Reference](docs/API.md) - All endpoints
- [Architecture](docs/ARCHITECTURE.md) - System design
- [Roadmap](ROADMAP.md) - Planned features

## 🐛 Troubleshooting

**Backend won't connect to database?**
```bash
# Make sure PostgreSQL is running
brew services start postgresql  # macOS
sudo systemctl start postgresql # Linux
```

**Flutter app can't reach backend?**
- Check backend is running on `http://localhost:5000`
- For Android emulator, use `10.0.2.2:5000`

**Port already in use?**
```bash
# Kill the process using port 5000
lsof -i :5000 | grep LISTEN | awk '{print $2}' | xargs kill -9
```

## 💡 Next Steps

1. Add more wallpapers to database
2. Customize colors and branding
3. Deploy backend to cloud
4. Publish to App Store/Play Store

## 📞 Need Help?

Check [SETUP.md](docs/SETUP.md) for detailed troubleshooting or open an issue on GitHub.

---

**Happy building! 🎨**
