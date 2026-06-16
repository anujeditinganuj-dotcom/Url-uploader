# URL Uploader Bot

A feature-rich Telegram bot for downloading videos from YouTube and 1000+ platforms using yt-dlp. Built with Pyrogram and MongoDB.

## 🌟 Features

### Core Features
- **Multi-Platform Support**: Download from YouTube, Instagram, Facebook, Twitter, TeraBox, and 1000+ sites
- **Format Selection**: Choose from available video formats and resolutions
- **Smart File Splitting**: Automatically splits large files (>1.75GB) for Telegram limits
- **Cookie Support**: Bypass login walls and age restrictions via Netscape cookie files
- **Custom Thumbnails**: Set custom thumbnails for uploaded videos
- **Custom Captions**: Add personalized captions to your downloads
- **Screenshot Generation**: Generate multiple screenshots from downloaded videos
- **Sample Video Creation**: Create 20-second preview clips

### User Management
- **Two-Tier System**: Regular and Student subscription plans
- **Daily Download Limits**: Free users get 20 downloads per day
- **Force Subscribe**: Require users to join channels before using the bot
- **User Database**: Track users, settings, and download history

### Admin Features
- **Cookie Management**: Add/delete global cookies for restricted site access
- **User Management**: Ban/unban users, add paid subscriptions
- **Subscription Control**: Set custom expiry dates for subscriptions
- **Broadcasting**: Send messages to all users
- **Statistics**: View paid users and their subscription details

---

## 📋 Prerequisites

- Python 3.11+
- MongoDB database
- FFmpeg installed on system
- Telegram Bot Token
- Telegram API credentials (API_ID and API_HASH)

---

## 🚀 Installation

### Method 1: Local / Termux

1. **Install dependencies**
```bash
pip install -r requirements.txt
```

2. **Install FFmpeg**

**Ubuntu/Debian:**
```bash
sudo apt update && sudo apt install ffmpeg
```

**Termux (Android):**
```bash
pkg install ffmpeg
```

3. **Configure environment variables**

Create a `.env` file in the root directory:
```env
API_ID=your_api_id
API_HASH=your_api_hash
BOT_TOKEN=your_bot_token
MONGODB_URI=mongodb+srv://...
DB_NAME=ytdl_bot_db
ADMINS=your_user_id
PAID_USERS=
FORCE_SUB_CHANNEL1=
FORCE_SUB_CHANNEL2=
```

4. **Run the bot**
```bash
python3 -m bot
```

### Method 2: Docker

```bash
docker build -t ytdl-bot .
docker run -d \
  -e API_ID=your_api_id \
  -e API_HASH=your_api_hash \
  -e BOT_TOKEN=your_bot_token \
  -e MONGODB_URI=your_mongodb_uri \
  -e ADMINS=your_user_id \
  --name ytdl-bot \
  ytdl-bot
```

### Method 3: Docker Compose

Create `docker-compose.yml`:
```yaml
version: '3.8'
services:
  bot:
    build: .
    environment:
      - API_ID=${API_ID}
      - API_HASH=${API_HASH}
      - BOT_TOKEN=${BOT_TOKEN}
      - MONGODB_URI=${MONGODB_URI}
      - ADMINS=${ADMINS}
    restart: unless-stopped
    depends_on:
      - mongodb
  mongodb:
    image: mongo:latest
    volumes:
      - mongodb_data:/data/db
    restart: unless-stopped
volumes:
  mongodb_data:
```

```bash
docker-compose up -d
```

---

## ⚙️ Configuration

### Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `API_ID` | Telegram API ID from my.telegram.org | ✅ |
| `API_HASH` | Telegram API Hash | ✅ |
| `BOT_TOKEN` | Bot token from @BotFather | ✅ |
| `MONGODB_URI` | MongoDB connection string | ✅ |
| `DB_NAME` | Database name | ❌ (default: `ytdl_bot_db`) |
| `ADMINS` | Space-separated admin user IDs | ✅ |
| `PAID_USERS` | Space-separated paid user IDs | ❌ |
| `FORCE_SUB_CHANNEL1` | First required channel username/ID | ❌ |
| `FORCE_SUB_CHANNEL2` | Second required channel username/ID | ❌ |

### `bot/config.py` defaults

```python
DEFAULT_UPLOAD_MODE = "video"       # 'video' or 'file'
DEFAULT_SPLIT_SETTING = True        # Auto-split large files
MAX_FILE_SIZE = int(1.75 * 1024**3) # 1.75 GB
TASKS = 20                          # Free user daily limit
```

---

## 📱 Commands

### User Commands

| Command | Description |
|---------|-------------|
| `/start` | Start the bot |
| `/help` | Show all commands |
| `/settings` | Configure upload mode, split, caption, screenshots |
| `/cookie` | Open cookie panel (view status) |
| `/caption <text>` | Set custom caption |
| `/clearcaption` | Remove custom caption |
| `/clearthumbnail` | Remove custom thumbnail |
| `/plans` | View subscription plans |
| `/upgrade` | Check your subscription status |

### Admin Commands

| Command | Description |
|---------|-------------|
| `/setcookie` | Upload a cookies.txt file (Netscape format) |
| `/delcookie` | Delete saved cookies |
| `/cookiestatus` | Check if cookies are active |
| `/broadcast <msg>` | Send message to all users |
| `/ban <user_id>` | Ban a user |
| `/unban <user_id>` | Unban a user |
| `/addpaid <user_id> <duration>` | Add paid subscription (`30d`, `3m`, `1y`) |
| `/removepaid <user_id>` | Remove paid status |
| `/paidusers` | List all active paid users |

---

## 🍪 Cookie Setup (Admin)

Cookies allow the bot to access login-restricted or age-gated content on YouTube, Instagram, Hotstar, etc.

### How to export cookies

1. Install **Cookie-Editor** extension in Chrome/Firefox
2. Log in to the target website (e.g. YouTube)
3. Click Cookie-Editor → **Export → Export as Netscape**
4. Save the file as `cookies.txt`

### How to add cookies to the bot

**Method 1 — Command:**
```
/setcookie
```
Then send the `cookies.txt` file.

**Method 2 — Cookie Panel:**
- Tap **🍪 Cookie Panel** from `/start` menu
- Tap **➕ Add Cookies**
- Send the `cookies.txt` file

### Supported formats
Both **tab-separated** and **space-separated** Netscape cookie files are accepted — the bot normalizes them automatically before passing to yt-dlp.

### Cookie file example
```
# Netscape HTTP Cookie File
.youtube.com	TRUE	/	FALSE	1750000000	LOGIN_INFO	your_value_here
.youtube.com	TRUE	/	TRUE	1750000000	SID	your_sid_here
```

---

## 💎 Subscription Plans

### Regular Plan
| Duration | Price |
|----------|-------|
| 1 Month | ₹30 |
| 3 Months | ₹85 |
| 6 Months | ₹160 |
| 1 Year | ₹300 |

### Student Plan *(verification required)*
| Duration | Price |
|----------|-------|
| 1 Month | ₹10 |
| 3 Months | ₹28 |
| 6 Months | ₹53 |
| 1 Year | ₹100 |

### Premium Features
- Unlimited downloads (no daily limit)
- File splitting for large videos
- Custom thumbnail & caption
- Screenshot generation
- Sample video creation

---

## 🗂️ Project Structure

```
Url-uploader-bot/
├── bot/
│   ├── __init__.py
│   ├── __main__.py      # Main bot logic & handlers
│   ├── config.py        # Configuration & env vars
│   ├── database.py      # MongoDB operations
│   ├── yt_helper.py     # yt-dlp download & cookie utilities
│   ├── web.py           # Flask keep-alive server
│   └── keep_alive.py    # Uptime ping
├── downloads/           # Temp download folder (auto-created)
├── .env                 # Environment variables (create this)
├── Dockerfile
├── requirements.txt
└── README.md
```

---

## 🔧 Troubleshooting

| Problem | Solution |
|---------|----------|
| Bot doesn't respond | Check BOT_TOKEN, verify MongoDB connection |
| Downloads fail | Ensure FFmpeg installed, check yt-dlp supports URL |
| Restricted content fails | Add cookies via `/setcookie` |
| Cookie file rejected | Must be Netscape format (tabs or spaces both OK) |
| File upload errors | Enable split in `/settings`, check disk space |
| Thumbnail fails | Ensure FFmpeg installed, try disabling thumbnail |

### Enable debug logging
```python
# In bot/__main__.py
logging.basicConfig(level=logging.DEBUG, ...)
```

---

## 📊 Database Schema

**Users**
```js
{ user_id, username, upload_mode, split_enabled, caption, caption_enabled,
  thumbnail, generate_screenshots, generate_sample_video, banned,
  is_paid, subscription_start, paid_expiry }
```

**URLs**
```js
{ url_id, url, user_id, status, timestamp }
```

**Daily Tasks**
```js
{ user_id, date, count }
```

**Settings** *(global)*
```js
{ key: "global_cookies", value: "<netscape cookie content>", updated_at }
```

---

## 📝 License

MIT License — see [LICENSE](LICENSE) for details.

## 👨‍💻 Author

**Anuj Kumar**
- Telegram: [@anujedits76](https://t.me/anujedits76)
- Support: [@anujeditbyak](https://t.me/anujeditbyak)
- Updates: [@log_ak_bots](https://t.me/log_ak_bots)

## 🙏 Built With

- [Pyrogram](https://github.com/pyrogram/pyrogram) — Telegram MTProto framework
- [yt-dlp](https://github.com/yt-dlp/yt-dlp) — Video downloader
- [FFmpeg](https://ffmpeg.org/) — Multimedia processing
- [MongoDB](https://www.mongodb.com/) — Database

---

> ⚠️ **Disclaimer**: For educational use only. Users are responsible for ensuring they have rights to download content.
