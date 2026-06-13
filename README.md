```markdown
# 🏀 SportsFire – Live Sports Streaming App

**SportsFire** is a next‑generation Android application that brings live cricket, football, and other sports streaming to your phone and Android TV. It features real‑time match updates, a rich channel list, dynamic banners, and an immersive player with quality controls – all powered by **Supabase** and **ExoPlayer**.

![Platform](https://img.shields.io/badge/platform-Android-green.svg)
![Min SDK](https://img.shields.io/badge/minSDK-24%20(Android%207.0)-blue.svg)
![License](https://img.shields.io/badge/license-MIT-lightgrey.svg)

---

## ✨ Features

| Category | Description |
|----------|-------------|
| 🎬 **Live Streaming** | Watch live cricket, football, and other sports with low‑latency HLS / MP4 streams. |
| 📺 **Live TV Channels** | Browse sports, news, and entertainment channels. |
| 🏟️ **Featured Matches** | Carousel of live & upcoming matches with auto‑scrolling. |
| 🔔 **Real‑time Updates** | Match status and channels refresh instantly via Supabase Realtime WebSocket. |
| 📱 **Android TV Support** | Leanback UI with D‑Pad navigation, focused cards, and fullscreen playback. |
| 🎚️ **Quality Selection** | Choose between Auto, 1080p, 720p, 480p, 360p, 240p (adaptive bitrate). |
| 🧹 **Cache Management** | Clear Glide image cache from settings. |
| 📊 **Live Viewer Counter** | Increment/decrement live viewers via Supabase RPC. |
| 🎨 **Glassmorphism UI** | Modern design with gradients, chips, and smooth animations. |

---

## 🛠️ Tech Stack

- **Language**: Kotlin
- **UI**: XML + Material Design, Leanback (for TV)
- **Networking**: Retrofit + OkHttp (Supabase REST API)
- **Realtime**: Custom WebSocket client (Supabase Realtime)
- **Video Player**: ExoPlayer with adaptive streaming support
- **Image Loading**: Glide (with disk caching)
- **Architecture**: MVVM (ViewModel + LiveData)
- **Dependency Injection**: Manual (no DI framework)
- **Data Persistence**: SharedPreferences + Gson

---

## 📦 Project Structure

```

app/src/main/java/com/injoysportsfire/bd/
├── adapter/                 # RecyclerView adapters (Banner, Channel, Match, Trending)
├── data/
│   ├── api/                 # Retrofit client, SupabaseApi interface, Realtime WebSocket
│   ├── model/               # Data classes (Banner, Channel, Match)
│   └── repository/          # ChannelRepository, MatchRepository
├── tv/                      # Android TV specific code
│   ├── TvBrowseFragment.kt
│   ├── TvMainActivity.kt
│   └── presenter/           # Leanback card presenters
├── ui/
│   ├── channels/            # ChannelsFragment + ViewModel
│   ├── home/                # HomeFragment + ViewModel
│   ├── player/              # PlayerActivity (ExoPlayer + gesture controls)
│   └── settings/            # SettingsFragment
├── utils/                   # Constants, DeviceUtils, ImageUrlUtil, NetworkUtils, SharedPrefManager
├── MainActivity.kt          # Bottom navigation + NavController
└── SportsfireApp.kt         # Application class for warm‑up requests

```

---

## 🚀 Getting Started

### 1. Prerequisites

- Android Studio Hedgehog or later
- JDK 11+
- A **Supabase** project (free tier works)
- Android device / emulator with API 24+ (Android 7.0)

### 2. Clone the repository

```bash
git clone https://github.com/yourusername/sportsfire.git
cd sportsfire
```

3. Set up Supabase

Create a Supabase project and set up the following tables:

Table matches

Column Type Description
id uuid (PK) Primary key
title text Match title (e.g. "IND vs AUS")
sport_type text "cricket", "football"
thumbnail_url text Image URL
stream_url text HLS / MP4 stream URL
status text "live" or "upcoming"
start_time timestamptz Match start time
created_at timestamptz Default: now()
current_viewers int Default: 0

Table channels

Column Type Description
id uuid (PK) Primary key
name text Channel name
logo_url text Logo image URL
stream_url text Stream URL
category text "sports", "news", "entertainment"
is_active boolean Enable/disable channel

Table banners

Column Type Description
id int (PK) Auto‑increment
title text Banner title
image_url text Banner image URL
stream_url text Link to stream / match
is_active boolean Enable/disable banner
display_order int Sort order (ascending)

RPC Functions (PostgreSQL)

Create two functions to safely update current_viewers:

```sql
-- Increment
CREATE OR REPLACE FUNCTION increment_live_viewer(match_id text)
RETURNS void AS $$
BEGIN
  UPDATE matches
  SET current_viewers = current_viewers + 1
  WHERE id = match_id::uuid AND status = 'live';
END;
$$ LANGUAGE plpgsql;

-- Decrement
CREATE OR REPLACE FUNCTION decrement_live_viewer(match_id text)
RETURNS void AS $$
BEGIN
  UPDATE matches
  SET current_viewers = current_viewers - 1
  WHERE id = match_id::uuid AND current_viewers > 0;
END;
$$ LANGUAGE plpgsql;
```

⚠️ Then enable Realtime for matches and channels tables in the Supabase dashboard.

4. Configure API keys

Open utils/Constants.kt and replace the placeholder values with your Supabase credentials:

```kotlin
const val SUPABASE_URL = "https://YOUR_PROJECT.supabase.co"
const val SUPABASE_ANON_KEY = "your-anon-key"
```

Do not commit your keys to public repositories. Use local properties or secrets.

5. Build & Run

· Open the project in Android Studio.
· Sync Gradle (use File → Sync Project with Gradle Files).
· Connect a device or start an emulator.
· Press Run (green triangle).

---

📺 Android TV Build

The app automatically detects TV devices (using UiModeManager) and switches to Leanback UI.
To test on a TV emulator:

1. Create an AVD with Android TV (1080p) skin.
2. Run the app – the launcher will show the TvMainActivity instead of the mobile bottom‑nav layout.

---

🎮 Player Controls

Gesture / Key Action
Tap (mobile) / OK (TV) Show/hide controls
Double‑tap left/right Seek ±10 seconds
D‑Pad left/right Seek ±10 seconds
D‑Pad up Show controls
Play/Pause button Play / pause stream
Back / B button Exit player
Quality icon Open resolution picker

---

🧪 Known Limitations

· Stream sources must be CORS‑enabled and support HTTPS.
· No built‑in DRM or license handling.
· Real‑time WebSocket may reconnect with a 5‑second delay on network loss.
· View counter RPC calls are fire‑and‑forget – no retry on failure.

---

🛠️ Troubleshooting

Issue Solution
tag name can't be blank (GitHub Release) Create a Git tag first: git tag v1.0 && git push origin v1.0
No streams play Check stream URL in Supabase – must be a direct .m3u8 or .mp4 URL.
Banners not showing Verify is_active = true and display_order in Supabase.
WebSocket disconnects Ensure Supabase Realtime is enabled for the tables.

---

📄 License

This project is licensed under the MIT License.
You are free to use, modify, and distribute it with attribution.

---

🙏 Acknowledgements

· ExoPlayer by Google
· Supabase – Open‑source Firebase alternative
· Glide for image loading
· Material Design Components

---

📬 Contact

For issues or suggestions, please open an issue on GitHub or reach out to the development team.

Enjoy streaming with SportsFire! 🔥

```
