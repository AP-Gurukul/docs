# Architecture & Security Blueprint

## 🔐 Team Permissions & Security

Security is critical to the AP-Gurukul ecosystem. Our primary goal is to **protect the backend code, Firebase Admin credentials, and database schemas** while allowing frontend engineers to see API endpoints for integration purposes.

### Teams

| Team Name | Purpose | Permissions |
|-----------|---------|-------------|
| **`@core-maintainers`** | Founders / Lead Architects | `Admin` on all repositories. Can merge to main and modify settings. |
| **`@backend-team`** | Backend Developers | `Write` access to `backend-api` and `firebase-functions`. |
| **`@frontend-web`** | Web App Developers | `Write` access to `web-app`. Isolated from backend code. |
| **`@frontend-mobile`**| Mobile Developers | `Write` access to `mobile-app`. Can view `web-app` (open-source) but isolated from backend code. |
| **`@telegram-team`** | Telegram App Devs | `Write` access to `telegram-web-app`. Isolated from backend code. |
| **`@admin-team`** | Internal Tools Devs | `Write` access to `admin-portal`. Has `Read` access to all frontend & backend repos for full context. |
| **`@devops`** | DevOps / Infrastructure | `Write` access to `infrastructure`. `Read` access to all repos. |

**Repository Visibility (Open Source vs Private)**
- **Public Repositories** (`web-app`, `mobile-app`, `telegram-web-app`): Since these are public, **all developers (e.g., mobile developers) can naturally see the web app code** and vice versa. Open source contributors can fork these repos and submit PRs.
- **Private Repositories** (`backend-api`, `admin-portal`, `infrastructure`): These are strictly protected. Mobile and web developers **do not** have read access to the backend or admin code to prevent security leaks. Only backend developers and admins can view backend code.

---

## 🐳 Docker Orchestration

The `infrastructure` repository contains a `docker-compose.yml` file. This acts as the glue that holds the system together.

### The Virtual Network (`apgurukul`)
When `docker compose up` is run, Docker creates an isolated virtual network named `apgurukul`. All containers sit inside this network.

This means:
- The Web App can talk to the Backend API internally via `http://backend-api:4000`
- The Backend API can talk to Redis internally via `redis://redis:6379`
- You can talk to any of them from your browser via `http://localhost:<PORT>`

### Hot Reloading (Volume Mounts)
Inside `docker-compose.yml`, you will see:
```yaml
volumes:
  - ../backend-api/src:/app/src
```
This tells Docker: *"Take the `src` folder on Praneeth's Mac, and mount it over the `/app/src` folder inside the Linux container."* 
Because of this, when you save a file in VS Code, `ts-node-dev` or `Vite` inside the container instantly sees the change and restarts the server. You get the stability of Docker with the speed of local development.
