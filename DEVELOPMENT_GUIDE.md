# Development & Contribution Guide

Welcome to the development guide! Here are the rules for pushing code.

## 🌿 Branching Strategy (Git Flow)

We strictly use a structured branching model. **Never push directly to `main`.**

1. **`main`**: Represents Production. Highly protected.
2. **`develop`**: Represents Staging. Where features are integrated together.
3. **`feat/*`**: Feature branches (e.g., `feat/add-login-screen`).
4. **`fix/*`**: Bug fixes (e.g., `fix/header-alignment`).

### Creating a Feature
```bash
git checkout main
git pull
git checkout -b feat/your-feature-name
```

### Committing Code
We use [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/):
- `feat: added user authentication` (Adds a new feature)
- `fix: resolved crashing map` (Fixes a bug)
- `docs: updated README instructions` (Changes to documentation)
- `chore: updated dependencies` (Maintenance work)

### Creating a Pull Request (PR)
When your code is ready:
1. Push your branch to GitHub.
2. Open a Pull Request from `feat/your-feature-name` to `main`.
3. CI/CD pipelines will automatically run linting and tests.
4. Require at least 1 approval from a `@core-maintainer` before merging.

---

## 🤝 Contribution Workflows

Because AP-Gurukul operates a hybrid open-source model, the workflow depends on who you are and what you are building.

### Open-Source Contributors (Public Repos)
*Applies to: `web-app`, `mobile-app`, `telegram-web-app`*
1. **Fork** the repository to your personal GitHub account.
2. Create a new branch (`feat/your-feature`) in your fork.
3. Push changes to your fork and open a **Pull Request** to our `main` branch.
4. An internal `@core-maintainer` will review and merge it. 
*(Note: As an open-source contributor, you will not have access to the `backend-api`. You must test against the provided mock APIs or public staging endpoints).*

### Internal Private Team (Backend & Admin)
*Applies to: `backend-api`, `admin-portal`, `infrastructure`*
1. Clone the repository directly (no forking required).
2. Create a branch locally (`feat/internal-feature`).
3. Commit and push directly to the AP-Gurukul repository.
4. Open an internal Pull Request for review by another internal developer.

---

## 🛠 Adding a New Package/Dependency
Since we are using Docker with mounted volumes, adding an `npm` package requires a specific workflow.

**Wrong Way:**
Running `npm install moment` on your Mac. The package is downloaded to your local `node_modules`, but the Docker container might crash because it's running Linux and the `node_modules` sync can break binary dependencies.

**Right Way:**
Run the install command *inside* the running Docker container:
```bash
docker compose exec backend-api npm install moment
```
This installs the package inside the container, updates `package.json`, and safely synchronizes it back to your Mac!
