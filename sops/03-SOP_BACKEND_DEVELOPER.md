# SOP 3: Internal Backend Developer

## 📌 Role Overview
As a backend developer (`@backend-team`), you are the gatekeeper of the AP-Gurukul data. You have access to the highly secure `backend-api` and `firebase-functions` repositories. Your primary job is to write the Express API that serves the frontends and maintain the Firebase database.

## 🛠 Required Tools
- Docker & Docker Compose
- Node.js (v20+)
- Postman / Insomnia (for API testing)

## 🔄 Daily Workflow

### 1. Setting Up the Local Environment
You must use Docker to run the backend safely.
1. Clone the infrastructure and backend repositories:
   ```bash
   git clone https://github.com/AP-Gurukul/infrastructure.git
   git clone https://github.com/AP-Gurukul/backend-api.git
   ```
2. Ask the Core Maintainer for the `.env` file credentials. Place them in `infrastructure/.env`.
3. Boot the system:
   ```bash
   cd infrastructure
   docker compose up -d
   ```
   *Your backend API is now running locally at `http://localhost:4000`.*

### 2. Developing Features
Because of Docker Volume Mounts, you do not need to restart the server manually.
1. Open the `backend-api` folder in your code editor.
2. Create a branch: `git checkout -b feat/new-auth-route`.
3. Modify a file (e.g., `src/index.ts`). The Docker container will automatically detect the save and restart the Express server via `ts-node-dev`.

### 3. API Contract & Communication
Whenever you create or modify an API endpoint:
1. You **MUST** update the API Documentation (Swagger/Postman).
2. If the database schema changes, you **MUST** update the TypeScript interfaces in the `shared-types` repository and publish a new version so the frontend teams don't break.
3. Notify the `@frontend-web` and `@frontend-mobile` teams of the change.

### 4. Pushing Code
1. Push your branch to GitHub.
2. Open a Pull Request. **All backend PRs require review from a Core Maintainer** before merging, due to the high security risk.
