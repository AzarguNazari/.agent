---
name: secure-coding
description: >-
  Security standards for input validation, authentication, cryptography, logging,
  and configuration when implementing or reviewing sensitive code paths.
---

## Trigger
Use when implementing input handling, authentication, authorization, encryption, error handling, configuration, or any security-sensitive code path.

## Rules by Category

### Input Validation
- Validate ALL incoming values (REST, HTTP, messages) — never assume client pre-validated
- For plain string inputs: define allowlist of characters + max content size
- Reject unexpected input early (guard clause pattern)

### Output Encoding
- Always encode output matching the output format context (HTML → HTML-encode, JSON → JSON-safe, etc.)
- Prevents XSS

### Authentication & Session
- Always use MFA where environment supports it
- Enforce strong passwords
- Session timeouts: as short as acceptable
- Implement brute-force prevention
- HTTP session ≠ login session — a valid HTTP session does not imply a valid login session

### Access Control
- Least privilege: users get only permissions needed for their task
- All access decisions must be enforced server-side
- Log and monitor access attempts
- Product management defines precise permission requirements — raise concerns if requirements seem missing or ineffective

### Cryptography
- No MD5 for hashing (weak) — use SHA-512 or stronger
- Use strong, industry-accepted encryption algorithms for data at rest and in transit

### Error Handling & Logging
- Exceptions must not be silently swallowed (see logging-conventions/SKILL.md)
- No personal/sensitive data in exception messages or logs
- Use appropriate log levels (see logging-conventions/SKILL.md)
- Avoid overly verbose logs that obscure real issues

### Communication Security
- All inter-service and client communication via SSL/TLS
- Unsecured communication requirements → escalate, do not implement

### Configuration Secrets
- No secrets (passwords, API keys, tokens) in code repositories or packaged artifacts
- Always use the designated secrets management tool
- `.editorconfig` and config files are fine; credentials are not

## Output format
When generating code in any security-relevant area, apply all rules above. Flag any violation with `// SECURITY VIOLATION: <category>`.
