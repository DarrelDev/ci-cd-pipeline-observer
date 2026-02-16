# 📡 GitHub Release Observer & Telegram Bridge

An automated observability engine designed to bridge the gap between GitHub Releases and external messaging platforms. This system monitors software releases in real-time and pushes instant notifications via API integration.

> **Architecture:** Event-Driven Polling | **Stack:** GitHub Actions, REST API, State Caching

## ⚙️ How It Works (The Engineering Logic)

This engine implements a **Stateful Monitoring System** within a stateless environment (GitHub Runners) using a **Rolling Cache Mechanism**.

1.  **State Restoration:** Retrieves the last processed `tag_name` (e.g., v27.0) from the ephemeral cache layer.
2.  **Differential Analysis:** Queries the target repository's Release API to compare the latest stable version against the cached state.
3.  **Asset Processing:** If a new version is detected, it parses JSON payloads to extract download links and converts file sizes from bytes to MB automatically.
4.  **API Dispatch:** Constructs a structured Markdown payload and delivers it to the end-user via Telegram Bot API in <2 seconds.
5.  **State Persistence:** Serializes the new version tag to prevent duplicate alerts in future schedules.

## 🛠️ Technical Implementation

- **Smart Caching:** Prevents spam/duplicate notifications using atomic cache locks.
- **Data Transformation:** Auto-converts raw bytes to human-readable formats (MB).
- **Zero-Server Infrastructure:** Runs entirely on GitHub Actions runners (Cron Job), requiring no VPS or external database.

## 🚀 Deployment

1. **Fork this repository.**
2. **Set Repository Secrets:**
   - `TELEGRAM_TOKEN`: Your BotFather token.
   - `TELEGRAM_TO`: Your Channel/Chat ID.
3. **Configure Target:**
   - Edit `env.TARGET_REPO` in `.github/workflows/monitor.yml` to any public repository you want to watch (e.g., `topjohnwu/Magisk`).

---

© 2026 DarrelDev. Designed for DevOps efficiency.
