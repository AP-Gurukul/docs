# AP-Gurukul Architecture Blueprint

AP-Gurukul is an open-source educational platform consisting of multiple specialized frontend clients that all connect to a unified API.

## 🏢 Platform Components

| Repository | Purpose | Description |
|-----------|---------|-------------|
| **`web-app`** | Primary Web Portal | Next.js application representing the student-facing educational platform. |
| **`mobile-app`** | Native Mobile App | Cross-platform mobile application built with React Native. |
| **`telegram-web-app`** | Telegram Mini App | Vite + React application optimized to run inside Telegram Messenger. |
| **`shared-ui`** | Design System | Reusable React components and design system (buttons, typography, theme) used across all clients. |

## 🔄 The Sync Ecosystem

At the core of AP-Gurukul is a unified cloud database.
- **Universal Authentication:** A user logs in via Gmail (Google Auth). Because all our frontend clients point to the same backend API, a user can start a quiz on the `web-app`, pause, and resume it on the `mobile-app` using the same login.
- **Real-Time Data:** When new subjects, questions, or resources are published by our core administration team, they are instantly beamed via WebSocket/API to all open-source frontends simultaneously.

## 🔐 Open Source Philosophy

We believe in building the future of education together. Our frontends (`web-app`, `mobile-app`, `telegram-web-app`, `shared-ui`) are 100% open-source. 
Developers worldwide can fork these repositories, run them locally against our public Staging API, and submit Pull Requests to improve the user interface, add new features, or fix bugs.
