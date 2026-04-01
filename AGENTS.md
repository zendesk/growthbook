# GrowthBook

Open source feature flagging and A/B testing platform. The monorepo contains a Next.js front-end, Express.js back-end, a Python stats engine, JavaScript/React SDKs, and shared TypeScript utilities. See `package.json` for Node version requirements and `packages/stats/pyproject.toml` for Python requirements.

## Setup & Commands

```bash
# Install all dependencies
yarn

# Initial build (builds SDKs, shared package, and stats engine)
yarn setup

# Start MongoDB (Docker)
docker run -d -p 27017:27017 --name mongo \
  -e MONGO_INITDB_ROOT_USERNAME=root \
  -e MONGO_INITDB_ROOT_PASSWORD=password mongo

# Start full app in dev mode (front-end + back-end + stats)
yarn dev

# Start only back-end in dev mode
yarn dev:back-end

# Run all tests (all packages)
yarn test

# Run back-end tests only
yarn workspace back-end test

# Run front-end tests only
yarn workspace front-end test

# Run stats tests only
yarn workspace stats test

# Lint (TypeScript)
yarn lint:ci

# Lint (Python stats engine)
yarn workspace stats lint:ci

# Type-check all packages
yarn type-check

# Format code
yarn pretty

# Regenerate OpenAPI types (run after changing openapi spec)
yarn generate-api-types

# Regenerate docs SDK info (run after SDK changes)
cd docs && yarn && yarn gen

# Build all packages
yarn build

# Build dependency packages only (SDKs + shared)
yarn build:deps
```

## Environment Configuration

```bash
# Copy and edit env files before running
cp packages/back-end/.env.example packages/back-end/.env.local
cp packages/front-end/.env.example packages/front-end/.env.local
```

Key back-end env vars: `JWT_SECRET`, `ENCRYPTION_KEY`, `MONGODB_URI`, `APP_ORIGIN`.

## Package Guide

| Package | Description |
|---------|-------------|
| `packages/front-end/` | Next.js UI (pages, components, hooks, services) |
| `packages/back-end/` | Express.js REST API (controllers, models, services, routers) |
| `packages/shared/` | TypeScript utilities and types shared between front/back |
| `packages/sdk-js/` | `@growthbook/growthbook` npm package |
| `packages/sdk-react/` | `@growthbook/growthbook-react` npm package |
| `packages/stats/` | Python stats engine (`gbstats` on PyPI — Poetry) |
| `docs/` | Docusaurus documentation site |

## Code Conventions

- **TypeScript** throughout front-end, back-end, and SDKs; use strict types, avoid `any`
- `wrapController()` wraps all async Express controllers for error handling
- Back-end request context (org, user, permissions) flows through the `Context` object from `services/context.ts`
- All config/secrets accessed via `packages/back-end/src/util/secrets.ts` — never `process.env` directly in controllers
- RBAC enforced server-side; use `context.permissions.throwIfCannotXxx()` for permission checks
- Models live in `packages/back-end/src/models/` and extend `BaseModel`; use the model class methods, not raw Mongoose queries in services
- `enterprise/` directories in front-end, back-end, and shared contain commercial-licensed code — avoid unintentional modifications there
- Python code follows `black` formatting and `flake8` linting with `pyright` type checking

## Testing

- **Back-end**: Jest — test files in `packages/back-end/test/`, mirror the `src/` structure
- **Front-end**: Vitest — test files in `packages/front-end/test/`
- **Stats**: Pytest — test files in `packages/stats/tests/`
- Run `yarn setup` before stats tests (installs Poetry virtualenv)
- Pre-commit hook runs `yarn lint` automatically; stats lint requires Poetry env

## Do

- Use `wrapController()` for all new Express controller files
- Use the `Context` object for auth, permission checks, and org-scoped data access
- Access configuration via `packages/back-end/src/util/secrets.ts`
- Put shared TypeScript types/utilities in `packages/shared/src/`
- Follow the router → controller → service → model layering in `back-end`
- Run `yarn generate-api-types` after changing OpenAPI specs
- Run `cd docs && yarn gen` after SDK capability changes

## Don't

- Don't call `process.env` directly in controllers or services — use `secrets.ts`
- Don't modify `enterprise/` directories without explicit intent
- Don't skip `wrapController()` on new controllers (unhandled promise rejections)
- Don't bypass the `Context` permission system for authorization
- Don't add raw Mongoose queries in service files — use model class methods
- Don't commit changes to `generated/spec.yaml` without running `yarn generate-api-types`
- Don't run `yarn dev` without the Poetry virtualenv set up (stats engine will fail)

## Architecture

See `ARCHITECTURE.md` for system architecture, component map, and design decisions.

## Security

See `SECURITY.md` for mandatory security requirements, prohibited patterns, and escalation triggers.

## Safety & Permissions

Allowed without approval:
- Read/list files
- Run single-file tests and linters
- Run type-checker on specific files

Ask before:
- Installing or removing packages
- Deleting files or directories
- Running full build or test suite
- Modifying CI/CD configuration
- Pushing to remote branches
- Modifying `enterprise/` directories

## PR & Commit Guidelines

- PR titles may use conventional commit format: `type(scope): description` (e.g., `feat(dashboards): add default view`, `fix(api): correct metric defaults`)
- Types: `feat`, `fix`, `docs`, `chore`, `refactor`
- Include Screenshots section for UI changes
- Include Testing section describing how to verify changes
- See `.github/pull_request_template.md` for full PR template

## References

- Architecture: `ARCHITECTURE.md`
- Security: `SECURITY.md`
- Contributing: `CONTRIBUTING.md`
- GrowthBook Docs: https://docs.growthbook.io
