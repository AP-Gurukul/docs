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
4. Require at least 1 approval from a core maintainer before merging.

---

## 🤝 Contribution Workflows

1. **Fork** the repository to your personal GitHub account.
2. Create a new branch (`feat/your-feature`) in your fork.
3. Push changes to your fork and open a **Pull Request** to our `main` branch.
4. An internal maintainer will review and merge it. 
*(Note: As an open-source contributor, you test against the provided public staging endpoints. You do not need to run local databases).*

---

## 🛠 Adding a New Package/Dependency
If you need to add an `npm` package, please ensure it is absolutely necessary. Keep our bundles small and performant. Run `npm install package-name` and ensure you commit the updated `package-lock.json` file.
