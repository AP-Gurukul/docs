# SOP 1: Open Source Contributor (Public Developer)

## 📌 Role Overview
As an open-source contributor, your goal is to help build out the frontend clients (`web-app`, `mobile-app`, `telegram-web-app`) and our shared design system (`shared-ui`). You do not have access to the backend or database, and you rely on public staging APIs.

## 🛠 Required Tools
- Node.js (v20+)
- Git
- VS Code (Recommended)
- React / React Native knowledge

## 🔄 Daily Workflow

### 1. Finding a Task
- Navigate to the GitHub repository you want to work on (e.g., `AP-Gurukul/web-app`).
- Look at the **Issues** tab for tasks labeled `good first issue` or `help wanted`.
- Comment on the issue asking to be assigned.

### 2. Setting Up Locally
1. Click the **Fork** button on the top right of the GitHub repository.
2. Clone your forked version to your local machine:
   ```bash
   git clone https://github.com/<YOUR_USERNAME>/web-app.git
   cd web-app
   ```
3. Install dependencies:
   ```bash
   npm install
   ```
4. Start the development server:
   ```bash
   npm run dev
   ```
   *(Note: The app is pre-configured to point to our public Staging API, so data will populate automatically without you needing a database).*

### 3. Writing Code
1. Create a branch for your feature:
   ```bash
   git checkout -b feat/my-new-button
   ```
2. Write your code. Ensure you are using components from `@ap-gurukul/shared-ui` when applicable.
3. Test your changes locally in the browser/emulator.

### 4. Submitting Your Work
1. Commit your code using Conventional Commits:
   ```bash
   git add .
   git commit -m "feat: added new glowing button to homepage"
   ```
2. Push to your fork:
   ```bash
   git push origin feat/my-new-button
   ```
3. Go to the original `AP-Gurukul/web-app` repository and click **Compare & pull request**.
4. Fill out the PR template explaining your changes.
5. Wait for a Core Maintainer to review your code. Do not ping reviewers directly; they will review it within 48 hours.
