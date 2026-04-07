[README.md](https://github.com/user-attachments/files/26528402/README.md)
# HD Downloads & Editing

A desktop app built with Electron for downloading videos, extracting audio, editing with AI assistance, and generating images — all in one place.

## Features

### Video Downloader
- Paste any URL from YouTube, TikTok, Twitter/X, Instagram, Twitch, SoundCloud, Reddit, and hundreds of other sites
- Choose quality: Best, 4K, 1080p, 720p
- Choose format: MP4, MKV, WebM
- Optional subtitle download (`.srt`)
- Batch download from a list of URLs or a `.txt` file
- Playlist detection — fetch and selectively queue individual videos
- Clipboard detection — auto-prompts when a video URL is copied

### MP3 Downloader
- Extract audio from any supported site
- Quality options: Best, 320kbps, 192kbps, 128kbps

### Studio (AI Video Editor)
- Load a local video or pull from download history
- Transcribe audio using Whisper
- AI assistant suggests edits based on your direction
- Long form and short form editing modes
- Remotion-powered video preview

### Image Generator
- Generate images using the Gemini API (auto-detects available model)
- Style presets: Photorealistic, Illustration, Cinematic, Anime, Abstract
- Aspect ratios: 1:1, 16:9, 9:16, 4:3
- Upload reference images to guide generation (supports multiple)
- Previously generated images shown in a picker for re-use as references
- Session image feed — same prompt groups into one card, click any image to view fullscreen
- Per-image save button, local estimated usage/cost tracker
- Live elapsed timer on each generating slot

### General
- Auto-updater (GitHub Releases)
- Download history with thumbnails
- Site authentication via browser cookies (Firefox/Edge recommended) or a cookies file
- yt-dlp update checker built in
- Diagnostics panel showing FFmpeg and yt-dlp paths

---

## Requirements

- **Windows** (x64)
- **Node.js** installed and on PATH (used by yt-dlp for full YouTube format support)
- A **Gemini API key** with billing enabled (for Image Generator)
- A **GitHub token** (for publishing releases)

---

## Setup

### 1. Clone and install

```bash
git clone https://github.com/PythonLuvr/HDVideoDownloader.git
cd HDVideoDownloader
npm install
```

### 2. Create a `.env` file

```env
GH_TOKEN=your_github_token
GEMINI_API_KEY=your_gemini_api_key
```

> The Gemini API key must have billing enabled. Free tier quota for image generation is 0. Get a key at [aistudio.google.com](https://aistudio.google.com).

### 3. Run in development

```bash
npm start
```

---

## Building & Publishing

Build the installer locally:

```bash
npm run dist
```

Build and publish to GitHub Releases:

```bash
npm run publish
```

> Make sure to bump the version in `package.json` before publishing if a release already exists for the current version.

Output: `dist/HD-Downloads-And-Editing-Setup-{version}.exe`

---

## Project Structure

```
├── main.js              # Electron main process — IPC handlers, downloads, API calls
├── renderer.js          # Renderer process — all UI logic
├── index.html           # App shell and view markup
├── style.css            # All styles
├── studio-player.jsx    # Remotion video player (built to studio-player.js)
├── package.json
└── .env                 # Secret keys (not committed)
```

---

## Tech Stack

| Layer | Tech |
|---|---|
| Desktop shell | Electron 41 |
| Video downloading | yt-dlp + ffmpeg-static |
| Video preview | Remotion + React 19 |
| Image generation | Google Gemini API |
| Auto-updates | electron-updater (GitHub Releases) |
| Build/package | electron-builder |
| JS bundler | esbuild |

---

## Notes

- **Chrome cookies do not work on Windows** due to DPAPI encryption. Use Firefox or Edge for browser-based site authentication.
- The Image Generator tracks estimated cost locally at **$0.04 per image** (Imagen 3 pricing). This is a local estimate only — check your actual usage at [ai.dev/rate-limit](https://ai.dev/rate-limit).
- Reference images sent to the Gemini API are included as inline base64 data in the request.
