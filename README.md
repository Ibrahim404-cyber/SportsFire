🏀 SportsFire – Live Sports Streaming App

SportsFire is a modern Android application for streaming live cricket, football, and other sports. It works on phones and Android TV, with real‑time updates, an adaptive video player, and a beautiful glassmorphism UI.

https://img.shields.io/badge/platform-Android-green.svg
https://img.shields.io/badge/minSDK-24%20(Android%207.0)-blue.svg

---

✨ Key Features

· Live match & channel streaming (HLS / MP4)
· Real‑time match status updates via WebSocket
· Android TV leanback interface with D‑Pad navigation
· Adaptive quality selection (Auto / 1080p / 720p / 480p / 360p / 240p)
· Live viewer counter
· Dynamic home banners (auto‑scroll)
· Channel filtering by category (Sports, News, Entertainment)
· Offline caching for channels & matches
· Clear image cache from settings
· Fully immersive player with gesture controls (double‑tap to seek)

---

🛠️ Tech Stack (Overview)

Layer Technology
Language Kotlin
UI XML + Material Design / Leanback
Networking Retrofit + OkHttp
Realtime Custom WebSocket client
Video ExoPlayer
Images Glide
Architecture MVVM (ViewModel + LiveData)
Storage SharedPreferences + Gson

---

🚀 Getting Started (Public)

1. Prerequisites

· Android Studio Hedgehog or newer
· JDK 11+
· A Supabase project (or any backend that provides a REST API and realtime events)

2. Clone & configure

```bash
git clone https://github.com/yourusername/sportsfire.git
```

Open the project and update the API endpoints in utils/Constants.kt:

```kotlin
const val SUPABASE_URL = "https://YOUR_PROJECT.supabase.co"
const val SUPABASE_ANON_KEY = "your-public-anon-key"
```

⚠️ Never commit real keys. Use local properties or environment variables.

3. Backend setup (example schema)

Create the following tables in your database:

· matches – id, title, sport_type, thumbnail_url, stream_url, status, start_time, current_viewers
· channels – id, name, logo_url, stream_url, category, is_active
· banners – id, title, image_url, stream_url, is_active, display_order

Enable realtime for the matches and channels tables.

4. Build & run

Connect a device / emulator (API 24+) and click Run in Android Studio.

---

📺 Android TV Mode

The app automatically switches to a Leanback interface when run on an Android TV device.
Test with an Android TV emulator (e.g., Android TV (1080p)).

---

🎮 Player Controls

Action Result
Tap / OK button Show/hide controls
Double‑tap left / right side Seek ±10 seconds
D‑Pad left / right Seek ±10 seconds
D‑Pad up Show controls
Play/Pause button Play / pause
Back button Exit player
Quality icon Open resolution menu

---

🧹 Clearing Cache

Go to Settings → Clear Cache to free up storage used by images.

---

📄 License

This project is licensed under the MIT License – feel free to use and modify it with attribution.

---

🙏 Credits

· ExoPlayer – Video playback
· Supabase – Backend & realtime
· Glide – Image loading

---

Enjoy streaming with SportsFire! 🔥
