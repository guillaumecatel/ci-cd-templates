# 🛠️ CI/CD Templates

This repository contains CI/CD configuration files for projects using **PNPM**, **Node.js**, and publishing **Docker** images to a private registry.

## 📁 Structure

```
pnpm/
├── .gitlab-ci.yml       # GitLab CI pipeline
├── ci.yaml              # GitHub Actions pipeline
└── README.md            # This file
```

---

## 🚀 Available CI/CD Pipelines

### ✅ Lint & Tests

Triggered on:

- all branches (`push`)
- all merge requests (GitLab) or pull requests (GitHub)
- all version tags

Runs the following steps:

- Read `.nvmrc` for Node.js version
- Install `pnpm@10`
- Lint code (`pnpm lint`)
- Format code (`pnpm format`)
- Run unit tests (`pnpm test:unit`)
- Run end-to-end tests (`pnpm test:e2e`)

---

### 🐳 Build & Push Docker (tags only)

Triggered only when a tag matching `v*` is pushed (e.g. `v1.0.0`)

Runs the following steps:

- Read `.nvmrc`
- Authenticate with private Docker registry
- Build Docker image using `docker buildx`
- Push image with:
  - Tag based on Git tag
  - Labels `org.opencontainers.image.version` and `source`

---

## 📦 GitHub Actions

Path: `pnpm/ci.yaml`
Should be moved to: `.github/workflows/ci.yml`

### Required Secrets

- `PRIVATE_REGISTRY_URL`
- `PRIVATE_REGISTRY_USERNAME`
- `PRIVATE_REGISTRY_PASSWORD`

---

## 🧱 GitLab CI

Path: `pnpm/.gitlab-ci.yml`
Should be moved to: `.gitlab-ci.yml`

### Required Variables

- `CI_REGISTRY`
- `CI_REGISTRY_USER`
- `CI_REGISTRY_PASSWORD`

---

## 🔧 Requirements

- A `.nvmrc` file with the Node.js version (e.g. `20.11.1`)
- A `Dockerfile` at the project root for building the Docker image

---

## 📘 Example `.nvmrc`

```
20.11.1
```

---

## 🧪 Example Test Commands

```json
"scripts": {
  "lint": "eslint .",
  "format": "prettier --check .",
  "test:unit": "vitest run",
  "test:e2e": "playwright test"
}
```

---

## 🤝 Compatibility

| Feature                         | GitHub Actions | GitLab CI |
| ------------------------------- | -------------- | --------- |
| Lint / Format / Tests           | ✅             | ✅        |
| Build & Push Docker (tag)       | ✅             | ✅        |
| Auto-read Node version (.nvmrc) | ✅             | ✅        |
| PNPM with corepack              | ✅             | ✅        |

---

## 📄 License

MIT © Guillaume CATEL
