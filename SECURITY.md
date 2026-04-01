# GrowthBook Security

This document has two parts: **AI agent security requirements** (mandatory guardrails for AI coding assistants) and the **vulnerability reporting policy** for GrowthBook.

---

## AI Agent Security Requirements

This section defines mandatory security principles for all AI coding assistants operating in this repository. Follow these requirements without exception.

### Core Security Mandate

Security is a first-class requirement. Every code suggestion must be evaluated against these guidelines. If a request would result in insecure code:

1. **Stop** and flag the security concern
2. **Explain** why it is problematic
3. **Propose** a secure alternative

AI-generated code requires human review before merging.

---

### Absolute Prohibitions

AI agents must **NEVER** do the following:

#### Secrets & Credentials
- Hardcode secrets, credentials, API keys, tokens, or passwords in source code
- Store secrets in version control (`.env` files with real values)
- Log, print, or expose secret values in any output
- Expose credentials in URLs, logs, or error messages
- Bypass `packages/back-end/src/util/secrets.ts` — never call `process.env` directly in controllers or services

#### Security Controls
- Disable or weaken security controls (`verify=False`, `ALLOW_ALL` CORS, disabled auth)
- Bypass the `Context` permission system or skip `context.permissions.throwIfCannotXxx()` checks
- Disable certificate validation
- Weaken JWT validation in `processJWT` or `authenticateApiRequestMiddleware`

#### Dangerous Code Patterns
- Build SQL or shell commands via string interpolation from user input
- Use `eval()` or `exec()` with user-supplied input
- Implement custom cryptography
- Use deprecated algorithms (MD5, SHA-1, DES, RC4, ECB mode)

#### Data Exposure
- Log sensitive data (PII, credentials, tokens, customer data, experiment results)
- Expose stack traces or internal paths to end users
- Transmit sensitive data over unencrypted channels
- Return raw MongoDB errors to API clients

---

### Required Security Patterns

#### Secret Access — Back-End

```typescript
// Correct: use secrets.ts
import { JWT_SECRET, ENCRYPTION_KEY } from "back-end/src/util/secrets";
```

```typescript
// NEVER: direct env access in controllers/services
const secret = process.env.JWT_SECRET;
```

#### Permission Checks

```typescript
// Correct: use Context permissions
context.permissions.throwIfCannotCreateMetric(project);
```

```typescript
// NEVER: skip authorization or roll your own check
if (user.role === "admin") { /* bypass */ }
```

#### Input Validation — API

```typescript
// Correct: validate with Zod schema before using input
const body = req.body as MySchema; // after middleware validation
```

```typescript
// NEVER: use raw user input in queries or commands without validation
const result = await db.find({ name: req.body.name });
```

#### Logging

```typescript
// Correct: structured logging without sensitive values
logger.info({ userId: user.id, action: "login" }, "User logged in");
```

```typescript
// NEVER: log tokens, passwords, or customer data
logger.debug({ token: authToken, password: req.body.password }, "Auth attempt");
```

---

### Security Requirements by Domain

#### Authentication
- JWT validation is handled centrally by `processJWT` and `authenticateApiRequestMiddleware` — do not bypass
- JWT secrets must come from `secrets.ts` (`JWT_SECRET`)
- SSO/OpenID Connect configuration lives in `services/auth/` — do not add ad-hoc auth paths
- Rate-limit authentication endpoints

#### Authorization
- All permission checks must use the `Context` object (`context.permissions.*`)
- Enforce access control server-side; never rely on front-end visibility alone
- Audit logs (`models/AuditModel`) must be written for sensitive mutations
- Enterprise RBAC in `enterprise/` — do not duplicate role logic outside that directory

#### Session Management
- Session tokens must be regenerated after privilege changes
- Use secure cookie attributes (`Secure`, `HttpOnly`, `SameSite`) — see `util/cookie.ts`
- Never store session state outside MongoDB or approved session stores

#### Data Protection
- Sensitive data source credentials encrypted using `ENCRYPTION_KEY` via `services/datasource.ts`
- Encrypt sensitive data at rest (KMS-managed keys in cloud deployments)
- Enforce HTTPS/TLS for all data in transit
- Redact PII and credentials from logs
- The `enterprise/` code may handle customer PII — be especially careful there

#### Cryptography

| Use Case | Approved | Forbidden |
|----------|----------|-----------|
| Password hashing | bcrypt, Argon2, PBKDF2 | MD5, SHA-1, unsalted hashes |
| Integrity hashing | SHA-256, SHA-3 | MD5, SHA-1 |
| Symmetric encryption | AES-256-GCM | DES, 3DES, AES-ECB |
| Asymmetric encryption | RSA-2048+, ECDSA | RSA < 2048 bits |
| TLS | TLS 1.2+ | SSLv2/3, TLS 1.0/1.1 |

#### Communication Security
- All API calls must use HTTPS — no HTTP fallback
- Validate webhook signatures (HMAC) before processing (`events/` handlers)
- Apply rate limiting and timeouts on outbound calls
- Validate webhook payloads against schema before processing

#### Error Handling
- Use `wrapController()` for all Express controllers — never expose raw errors to clients
- Restrict detailed errors to internal logs (pino/Sentry) — return generic messages to users
- Log security events (auth failures, authorization denials) without sensitive values

#### Infrastructure
- Apply least privilege to IAM roles — no `*` actions/resources
- S3 buckets default to private (`UPLOAD_METHOD=s3` paths)
- Use environment variables / Secrets Manager for sensitive values
- Enable encryption at rest for MongoDB

---

### When to Stop and Escalate

Stop, explain the concern, and involve a human reviewer if a task requires:

- Bypassing or weakening JWT validation or the `Context` permission system
- Exposing customer experiment data or PII
- Disabling certificate validation or TLS
- Using deprecated cryptographic algorithms
- Accessing or modifying the `enterprise/` license enforcement code
- Generating SQL queries from unvalidated user input in data source integrations
- Disabling Sentry, audit logging, or security event logging

---

### Security Testing

When generating features, include:

- Unit tests for permission checks and authorization enforcement
- Negative test cases for unauthorized access attempts
- Input validation boundary tests for API endpoints
- Tests for data source credential encryption/decryption

---

## Vulnerability Reporting Policy

**IMPORTANT: Do not file public GitHub issues for security vulnerabilities.**

Report security vulnerabilities by emailing: **security@growthbook.io**

We appreciate responsible disclosure and will make every effort to acknowledge contributions.

### Patching and Releasing

1. The security team investigates the vulnerability and determines scope and criticality
2. If valid, the code will be patched and released as quickly as possible
3. Affected users will be notified; a public security advisory will be published via [GitHub Security Advisories](https://github.com/growthbook/growthbook/security/advisories)

**Questions?** Reach out via [Slack](https://slack.growthbook.io) or email security@growthbook.io.
