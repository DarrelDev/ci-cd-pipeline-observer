# 📡 CI/CD Pipeline Observer & Notification Bridge



An automated observability engine designed to bridge the gap between GitHub Actions and external messaging platforms. This system monitors build artifacts in real-time and pushes status updates via API integration, eliminating the need for manual polling.



> **Architecture:** Event-Driven Polling | **Stack:** GitHub Actions, REST API, State Caching



## ⚙️ How It Works (The Engineering Logic)



This is not just a simple forwarder. It implements a **Stateful Monitoring System** within a stateless environment (GitHub Runners) using a **Rolling Cache Mechanism**.



1.  **State Restoration:** The workflow retrieves the cryptographic hash (SHA) of the last processed commit from the ephemeral cache layer.

2.  **Differential Analysis:** It queries the target repository's API to compare the latest successful build against the cached state.

3.  **Artifact Inspection:** If a new build is detected, the engine validates the integrity and existence of build artifacts (binaries).

4.  **API Dispatch:** Using the Telegram Bot API, it constructs a structured payload containing build metadata and download links, delivering it to the end-user in <2 seconds.

5.  **State Persistence:** The new state is serialized and stored with a unique dynamic key to prevent race conditions in future scheduled runs.



## 🛠️ Technical Implementation



- **Concurrency Control:** Prevents duplicate notifications using atomic cache locks.

- **API Efficiency:** Utilizes specific API filters (`status=success`) to minimize payload size and latency.

- **Zero-Server Infrastructure:** Runs entirely on GitHub Actions runners, requiring no VPS or external database.



## 🚀 Deployment



1. **Fork this repository.**

2. **Set Repository Secrets:**

&nbsp;  - `TELEGRAM\_TOKEN`: Your BotFather token.

&nbsp;  - `TELEGRAM\_TO`: Your Channel/Chat ID.

3. **Configure Target:**

&nbsp;  - Edit `env.TARGET\_REPO` in `monitor.yml` to the repository you want to watch.



---

© 2026 DarrelDev. Designed for DevOps efficiency.

