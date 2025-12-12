# Security

> **Scope:** OWASP guidelines, data protection, and authentication safety.
> **Objective:** Zero-trust architecture and data privacy.

## Core Security Principles (OWASP)

- **Trust No One:** Treat ALL input (params, headers, API responses, DB data) as untrusted. Validate explicitly.
- **Least Privilege:** The code should only request the permissions it absolutely needs.
- **Defense in Depth:** Do not rely on frontend validation alone. Backend validation is mandatory.

## Critical Implementation Rules

### Injection Prevention

- **SQL:** NEVER concatenate strings to build SQL queries. ALWAYS use Parameterized Queries, Prepared Statements, or an ORM.
- **OS Commands:** NEVER pass user input directly to system shell commands.
- **HTML/JS:** Context-aware encoding is mandatory when rendering user input to prevent XSS (Cross-Site Scripting).

### Data Protection

- **Secrets:** NEVER hardcode passwords, API keys, tokens, or connection strings in the source code. Use Environment Variables.
- **Sensitive Data Exposure:**
  - NEVER log PII (Passwords, Credit Cards, Auth Tokens).
  - NEVER return raw database errors or stack traces to the client in production. Return generic error messages (e.g., "An error occurred", not "Table 'users' missing").

### Authentication & Authorization

- **Broken Access Control:** Verify permissions on EVERY request. Just because a user is logged in doesn't mean they own the resource ID they are requesting.
- **Session Management:** Don't create your own session logic. Use established framework libraries.

### Secrets handling

- Make sure the environment variables are not exposed in the codebase, a .env file like should be ignored by the .gitignore file and never committed to the repository.
- Use a secrets manager to store sensitive data.

### Prompt injection prevention

- Use a prompt injection prevention library.
- Never use user input in the prompt without proper validation.

### Infra Security

- Enforce the use of HTTPS.
- Enforce the use of Web Application Firewalls (WAF).
- Enforce the use of SSO (Single Sign-On) in Kubernetes.

### Vulnerability Assessment

- Use a vulnerability assessment tool to scan the codebase for vulnerabilities provided by the same stack or language.
- Advice to use a vulnerability assessment tool to scan the codebase for vulnerabilities provided by the same stack or language.