# Linkdrop

**A free, ad-free video downloader for YouTube and Instagram.**

Linkdrop is a lightweight, open-source application for downloading individual public YouTube videos, YouTube Shorts, and Instagram video posts/Reels.

It offers two ways to run:

- **Windows Launcher** – Download and run Linkdrop locally with a simple double-click.
- **Self-Hosted Web App** – Run your own Linkdrop website using Docker or Node.js.

No subscriptions. No advertisements. No analytics. No paid APIs. No accounts required.

---

## ✨ Features

- **YouTube Support** – Download individual public videos and Shorts.
- **Instagram Support** – Download public video posts and Reels.
- **Video Downloads** – Select available resolutions, including 1080p and 720p when provided by the source.
- **Best Quality** – Automatically selects the highest available source resolution.
- **Audio Downloads** – Extract audio as MP3 at 320, 256, or 192 kbps.
- **Automatic Stream Merging** – Combines separate video and audio streams when necessary.
- **Live Progress** – Monitor download progress in real time.
- **Download Cancellation** – Cancel downloads whenever needed.
- **Recent Downloads** – Access recently completed downloads.
- **Browser Preferences** – Save preferences locally in your browser.
- **Temporary Storage** – Automatically cleans up temporary files.
- **Privacy Focused** – No analytics, advertising, tracking, or platform-account access.

---

## 🖥️ Windows Installation

The easiest way to use Linkdrop is through the Windows launcher.

### Requirements

- Windows 10 or Windows 11 (64-bit).
- An internet connection for initial setup and downloading videos.

### Getting Started

1. Download the latest Linkdrop ZIP from the [GitHub Releases](../../releases) page.
2. Extract the entire ZIP file into a folder.
3. Open the extracted folder.
4. Double-click `Start-Linkdrop.cmd`.
5. Linkdrop will open automatically in your default browser.

### Downloading a Video

1. Paste a supported YouTube or Instagram link.
2. Click **Find video**.
3. Choose Video or Audio.
4. Select your preferred quality.
5. Click **Download**.
6. Save the completed file through your browser.

If your browser blocks automatic downloads, use the **Save** option under Recent downloads.

### Windows Launcher Commands

| File | Description |
| --- | --- |
| `Start-Linkdrop.cmd` | Starts Linkdrop and opens the browser. |
| `Stop-Linkdrop.cmd` | Stops the background server. |
| `Update-Engine.cmd` | Updates yt-dlp. |

The launcher downloads missing free dependencies during the first run:

- Node.js 24 LTS
- yt-dlp
- FFmpeg

Downloads are verified using SHA-256 checksums from official release sources. Administrator privileges are not required.

The dependencies are installed inside the application folder.

**Important:** Stop Linkdrop before running `Update-Engine.cmd`. Restart the application after updating.

---

## 🌐 Self-Hosting

Linkdrop includes its complete frontend and backend.

It is not publicly hosted by default. You can deploy your own instance using Docker or run it directly with Node.js.

### Option 1: Docker (Recommended)

Install [Docker](https://www.docker.com/) and Docker Compose.

Clone the repository:

```bash
git clone https://github.com/YOUR_USERNAME/Linkdrop.git
cd Linkdrop
```

Build and start the application:

```bash
docker compose up --build -d
```

Access the application using the local address configured by your deployment.

By default, the application is restricted to local access.

### Deploying with a Public Domain

To make your self-hosted instance accessible through a public domain:

1. Copy `.env.example` to `.env`.
2. Set your public origin:

```env
PUBLIC_ORIGIN=https://your-domain.example
```

3. Configure your domain to point to your server.
4. Set up an HTTPS reverse proxy, such as [Caddy](https://caddyserver.com/).
5. Configure the reverse proxy to forward requests to the Linkdrop server.
6. Preserve the original HTTP Host header.
7. Rebuild and restart:

```bash
docker compose up --build -d
```

The interface automatically switches from local-computer wording to server-hosted wording.

**Deployment notes:**

- Only the configured public origin is accepted.
- Use HTTPS for public deployments.
- Configure appropriate reverse-proxy rate limits and access controls.
- Anyone with access to the public URL can consume available download resources.
- Linkdrop does not include user accounts or authentication.
- Use one application instance per data directory.

Docker Compose sets a memory ceiling of 2 GB and a CPU ceiling of 2 CPUs.

### Option 2: Run Without Docker

Install the following:

- Node.js 22 or newer (Node.js 24 LTS recommended).
- FFmpeg, including `ffprobe`.
- `yt-dlp[default]`.

On Linux, install yt-dlp inside a Python virtual environment and add its `bin` directory to PATH.

Run the application:

```bash
node server.mjs
```

By default, the server listens only for local connections.

For network access, configure:

```env
HOST=0.0.0.0
PUBLIC_ORIGIN=https://your-domain.example
```

Use an HTTPS reverse proxy for public access.

---

## ⚙️ Configuration

Linkdrop supports the following environment variables:

| Variable | Default | Description |
| --- | --- | --- |
| `PORT` | Application default | Server port |
| `HOST` | Local interface | Server listen address |
| `PUBLIC_ORIGIN` | Unset | Exact allowed browser origin for web mode |
| `DATA_DIR` | `data` | Application temporary storage directory |
| `YTDLP_PATH` | Bundled executable or `yt-dlp` | yt-dlp executable path |
| `FFMPEG_PATH` | Bundled executable or `ffmpeg` | FFmpeg executable path |
| `MAX_FILE_MB` | `1024` | Maximum output file size in MB |
| `FILE_TTL_MINUTES` | `60` | Temporary file retention time |

Never point `DATA_DIR` at personal files or directories containing unrelated data.

---

## 📦 Supported Formats and Quality

### Video

- Available source resolutions.
- 1080p and 720p when provided by the source.
- Best quality automatically selects the highest available source resolution.
- Separate audio and video streams are merged when required.
- MP4 output.

Portrait video resolution is measured using its shorter side. For example, 1080 × 1920 is displayed as 1080p.

### Audio

MP3 output options:

- 320 kbps
- 256 kbps
- 192 kbps

Linkdrop uses the best audio offered by the source.

Selecting a higher output bitrate does not improve the original audio quality.

---

## 🔒 Privacy and Security

Linkdrop is designed to be simple and privacy-conscious.

- No advertisements.
- No analytics or tracking.
- No subscriptions.
- No paid APIs.
- No user accounts.
- No collection of browser credentials.
- No access to your YouTube or Instagram login cookies.
- Random HttpOnly session cookies isolate browser jobs and files.
- Same-origin checks help protect application endpoints.
- Temporary downloads are automatically cleaned up.

### Temporary File Management

- Temporary files expire after 60 minutes.
- Temporary files are cleared when the server restarts.
- Files already saved through your browser are not deleted by Linkdrop.
- Download history is stored in server memory and scoped to the browser's session cookie.

### Resource Limits

| Resource | Default Limit |
| --- | --- |
| Concurrent downloads | 2 |
| Concurrent metadata lookups | 4 |
| Maximum output file | 1 GB |
| Maximum source duration | 3 hours |
| Maximum download duration | 30 minutes |
| Recent items per browser | 30 |
| Total recent jobs | 100 |
| Metadata expiration | 15 minutes |

These limits help keep the application manageable. There is no daily paid download quota.

---

## ⚠️ Limitations

Linkdrop works with publicly accessible individual videos, but not every URL is guaranteed to work.

Downloads may fail when:

- A platform requires authentication.
- A video is region restricted.
- A video has been removed.
- A platform blocks or rate-limits requests.
- The source does not provide the requested quality.

The application does not bypass:

- Private accounts.
- DRM protection.
- Paywalls.
- Authentication requirements.
- Other access restrictions.

The following are not supported:

- Playlists and albums.
- Live streams.
- Bulk downloads.

Only download content that you own or have permission to save. Users are responsible for complying with applicable copyright laws and platform terms.

---

## 🧪 Development and Testing

Linkdrop is built with Node.js and a lightweight frontend.

There are no npm dependencies required for the Node server or frontend.

Run the tests:

```bash
node --test test/*.test.mjs
```

Check the server:

```bash
node --check server.mjs
```

Check the frontend:

```bash
node --check public/app.js
```

API lifecycle tests use a simulated external extractor with real HTTP requests, subprocesses, and filesystem operations.

These tests verify application behavior but do not guarantee that third-party platforms will allow a particular download.

See `VERIFICATION.md` for the actual testing results.

---

## 🛠️ Built With

| Technology | Purpose |
| --- | --- |
| [Node.js](https://nodejs.org/) | Application server |
| [yt-dlp](https://github.com/yt-dlp/yt-dlp) | Video extraction and downloading |
| [yt-dlp EJS](https://github.com/yt-dlp/yt-dlp/wiki/EJS) | JavaScript runtime support |
| [FFmpeg](https://ffmpeg.org/) | Audio/video conversion and stream merging |
| [Docker](https://www.docker.com/) | Self-hosted deployment |

### Third-Party Notices

See `THIRD-PARTY-NOTICES.md` and the runtime license files for third-party licenses and source locations.

---

## 📄 License

Linkdrop application source is released under the **MIT License**.

See [LICENSE](LICENSE) for details.

---

## 🤝 Contributing

Contributions, bug reports, and suggestions are welcome!

If you encounter a problem:

1. Check whether the video is publicly accessible.
2. Try updating yt-dlp.
3. Restart Linkdrop.
4. Check the application logs for errors.
5. Open a GitHub Issue with the relevant details.

Please do not include private account credentials, session cookies, or other sensitive information in bug reports.

---

## ⭐ Support the Project

If you find Linkdrop useful:

- Give the repository a ⭐ Star.
- Report bugs and suggest improvements.
- Share the project with others.
- Contribute code or documentation.

**Linkdrop — Simple. Free. Open Source. No Ads.**
