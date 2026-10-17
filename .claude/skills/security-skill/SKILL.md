---
name: security-skill
description: General security checklist for reviewing code, configuration, and projects. Use when the user asks for a security check, security checklist, or "is this secure?", wants to harden an app before launch, or is adding auth, user input handling, file uploads, secrets, or new dependencies.
---

# Security Skill

Work through this checklist against the code or project the user points at. Focus on what actually applies — skip sections that don't fit (e.g., no database means no SQL injection check) and say which ones you skipped.

## 1. Understand the scope

Before checking anything, work out:

- **What** is being reviewed: a single file, a feature, a diff, or the whole project
- **What it exposes**: public web app, internal tool, API, CLI, library
- **What data it handles**: personal data, credentials, payments, nothing sensitive

## 2. Checklist

**Secrets and credentials**
- [ ] No API keys, passwords, tokens, or private keys in source code or committed config
- [ ] `.env` and similar files are in `.gitignore`; an `.env.example` documents required variables without real values
- [ ] Secrets are not printed in logs, error messages, or sent to the client
- [ ] Check git history (`git log -p`) if a secret may have been committed before — removing it from the latest version is not enough; it must be rotated

**Input handling**
- [ ] All user input is validated on the server (type, length, format), not only in the browser
- [ ] Database queries use parameterized queries or an ORM — never string concatenation (SQL injection)
- [ ] Output is escaped for its context; no raw user input in HTML (XSS)
- [ ] No user input passed to shell commands, `eval`, or file paths without strict validation (command injection, path traversal)
- [ ] File uploads check type and size, store files outside the web root, and use generated filenames

**Authentication**
- [ ] Passwords are hashed with a slow algorithm (bcrypt, scrypt, argon2) — never stored in plain text or with MD5/SHA1
- [ ] Login has rate limiting or lockout against brute force
- [ ] Sessions/tokens expire, are invalidated on logout, and cookies are `HttpOnly`, `Secure`, and `SameSite`
- [ ] Password reset tokens are single-use and short-lived

**Authorization**
- [ ] Every protected endpoint checks permissions on the server
- [ ] Users can only access their own records — changing an ID in a URL or request doesn't expose someone else's data
- [ ] Admin functions are restricted and not just hidden in the UI

**Data protection**
- [ ] HTTPS everywhere; no sensitive data over plain HTTP
- [ ] Only necessary personal data is collected and stored
- [ ] Sensitive data is not in URLs (they end up in logs and browser history)

**Dependencies**
- [ ] Run the ecosystem's audit tool (`npm audit`, `pip-audit`, `bundle audit`, `cargo audit`, etc.) and address high/critical findings
- [ ] Dependencies are pinned via a lockfile that is committed
- [ ] No abandoned or unknown packages for security-critical tasks

**Configuration**
- [ ] Debug mode and verbose errors are off in production
- [ ] Security headers set where relevant: `Content-Security-Policy`, `Strict-Transport-Security`, `X-Content-Type-Options`
- [ ] CORS allows only the origins that need access — not `*` on authenticated APIs
- [ ] Default credentials changed; unused services, ports, and endpoints disabled

**Logging and errors**
- [ ] Errors shown to users are generic; details go to server logs only
- [ ] Security-relevant events (logins, failed logins, permission changes) are logged without logging secrets

## 3. Report findings

Lead with a one-line summary of the overall state. Then list issues by severity:

- **Critical** — exploitable now; exposes data or allows account takeover (e.g., committed secret, SQL injection)
- **High** — serious weakness likely to be exploited (e.g., missing authorization check)
- **Medium** — weakens defenses (e.g., missing rate limiting, weak headers)
- **Low** — hardening and best practice

For each: **where** (`file:line`), **what** the risk is in plain language, and **how to fix** it with a concrete code or config change. If nothing serious is found, say so plainly and list what was checked.

## 4. Rules

- Never print the full value of a secret you find — show only enough to identify it (e.g., first 4 characters).
- If a real secret was committed, tell the user to rotate it immediately, before anything else.
- Don't run exploit attempts against live systems; review code and configuration only.
