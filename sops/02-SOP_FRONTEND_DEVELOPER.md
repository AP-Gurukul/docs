# SOP 2: Internal Frontend Developer

## 📌 Role Overview
As an internal frontend developer (`@frontend-web`, `@frontend-mobile`, `@telegram-team`), you are a trusted member of the AP-Gurukul organization. You have direct Write access to the frontend repositories. You do NOT have read or write access to the backend API or database to ensure strict security.

## 🛠 Required Tools
- Node.js (v20+)
- Git (Authenticated via GitHub CLI or SSH)
- Expo (for Mobile Devs)

## 🔄 Daily Workflow

### 1. Setting Up Locally
Unlike open-source contributors, you do not fork the repositories.
1. Clone the repository directly:
   ```bash
   git clone https://github.com/AP-Gurukul/mobile-app.git
   cd mobile-app
   ```
2. Install dependencies and start the app:
   ```bash
   npm install
   npm run start
   ```

### 2. Communicating with the Backend
Because you cannot see the backend code, you must rely on the **API Documentation** (provided by the backend team via Swagger or Postman).
- If you need a new endpoint (e.g., you are building a new Profile page and need `/api/profile`), you must request it from the `@backend-team`.
- Do not attempt to guess API routes or database structures. Use the shared TypeScript models from `@ap-gurukul/shared-types`.

### 3. Writing and Pushing Code
1. Ensure your `main` branch is up to date:
   ```bash
   git checkout main
   git pull origin main
   ```
2. Create your feature branch:
   ```bash
   git checkout -b feat/profile-screen
   ```
3. Write your code and test it against the Staging API.
4. Commit and push directly to the AP-Gurukul repo:
   ```bash
   git push origin feat/profile-screen
   ```
5. Open a Pull Request on GitHub. Assign another internal frontend developer to review your code.

### 4. Code Review Duties
You are expected to review Pull Requests from Open Source Contributors. Ensure they are following the design system (`shared-ui`) and not introducing malicious code.
