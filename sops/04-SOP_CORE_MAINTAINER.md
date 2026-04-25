# SOP 4: Core Maintainer & Admin

## 📌 Role Overview
As an Admin/Core Maintainer (`@core-maintainers`), you have absolute control over the AP-Gurukul organization. You manage the GitHub permissions, the Firebase database, the API keys, and the final production deployments.

## 🛠 Required Tools
- Docker & Docker Compose
- Firebase Console Access
- GitHub Organization Admin Access

## 🔄 Daily Workflow

### 1. Managing Access & Permissions
- When a new internal developer is hired, go to `github.com/orgs/AP-Gurukul/teams`.
- Add them to the correct team (e.g., `frontend-mobile`). **Do not grant Admin access.**
- Ensure they sign any NDAs before providing them with internal `.env` credentials.

### 2. Testing the Ecosystem Locally
You are the only role that regularly needs to test the *entire* ecosystem at once to ensure a feature works end-to-end.
1. Clone **all** repositories.
2. Ensure your `infrastructure/.env` is loaded with production or staging Firebase keys.
3. Run `docker compose up -d --build`.
4. Test the flow: Open the Admin Portal (`localhost:5173`), create a dummy question, and immediately verify it appears on the Web App (`localhost:3000`) and Telegram App (`localhost:3001`).

### 3. Reviewing & Merging
You are the final gatekeeper for production.
- **Frontend PRs**: Ensure the UI matches the design system. Approve and merge.
- **Backend PRs**: *Scrutinize heavily.* Ensure no security rules are bypassed and that Firebase Admin privileges are not abused.
- **Open Source PRs**: Review for malicious code. Open-source code should never touch `process.env` directly.

### 4. Deploying to Production
*(To be automated via GitHub Actions)*
1. Merge approved code into `main`.
2. Ensure the Firebase Security Rules (`firestore.rules`) are synchronized with the Firebase Console.
3. Monitor the Vercel/Render dashboards for successful deployment.
