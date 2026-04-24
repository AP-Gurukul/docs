# AP-Gurukul Organization Documentation

Welcome to the AP-Gurukul Organization! This is a multi-repository ecosystem that includes an Admin Portal, a Web App, a Telegram Web App, and a shared Backend API.

## 🏢 Repositories Overview

We have divided the system into distinct repositories to allow specialized teams to work concurrently without merge conflicts, and to strictly separate public-facing applications from private, proprietary backend code.

### Private Repositories
- **[admin-portal](https://github.com/AP-Gurukul/admin-portal)**: React + Vite application for managing the organization's content, questions, and users.
- **[backend-api](https://github.com/AP-Gurukul/backend-api)**: Express + TypeScript REST API that serves data to all frontends. Connected directly to Firebase Admin.
- **[infrastructure](https://github.com/AP-Gurukul/infrastructure)**: Contains the orchestration code (Docker Compose, Nginx, CI/CD scripts).
- **[shared-types](https://github.com/AP-Gurukul/shared-types)**: Shared TypeScript definitions and data models.
- **[firebase-functions](https://github.com/AP-Gurukul/firebase-functions)**: Cloud Functions for background processing and database triggers.

### Public Repositories (Open Source)
- **[web-app](https://github.com/AP-Gurukul/web-app)**: Next.js application representing the student-facing educational platform.
- **[mobile-app](https://github.com/AP-Gurukul/mobile-app)**: React Native application for iOS and Android.
- **[telegram-web-app](https://github.com/AP-Gurukul/telegram-web-app)**: Vite + React application optimized as a Telegram Mini App.
- **[shared-ui](https://github.com/AP-Gurukul/shared-ui)**: Reusable React components and design system (buttons, typography, theme).

---

## 🚀 Quick Start

To run the entire ecosystem locally on your machine, you only need Docker.

1. **Clone all repositories** into the same parent folder:
   ```bash
   git clone https://github.com/AP-Gurukul/infrastructure.git
   git clone https://github.com/AP-Gurukul/admin-portal.git
   git clone https://github.com/AP-Gurukul/backend-api.git
   git clone https://github.com/AP-Gurukul/web-app.git
   git clone https://github.com/AP-Gurukul/telegram-web-app.git
   ```

2. **Navigate into the infrastructure folder**:
   ```bash
   cd infrastructure
   ```

3. **Start the ecosystem**:
   ```bash
   docker compose up -d --build
   ```

4. **Access the services**:
   - **Nginx Router**: `http://localhost:80`
   - **Web App**: `http://localhost:3000`
   - **Admin Portal**: `http://localhost:5173`
   - **Backend API**: `http://localhost:4000`
   - **Telegram App**: `http://localhost:3001`

Any code changes you make in the respective folders will automatically hot-reload in the Docker containers!

---

## 📖 Further Reading
- [Architecture & Permissions Guide](./ARCHITECTURE.md)
- [Development & Contribution Guide](./DEVELOPMENT_GUIDE.md)
