---
name: logging-conventions
description: >-
  Java Log4j2 setup, log levels, confidential data rules, HTTP status mapping,
  and Graylog/Sentry conventions.
---

## Trigger
Use when writing any logging statements in Java or reviewing code that logs data.

## Logger Setup (Java)
```java
private static final Logger LOGGER = LogManager.getLogger(MyClass.class);
// OR with Lombok:
@Log4j2
```
Framework: log4j2 only. SLF4J, Apache Commons Logging, JUL, JBoss: forbidden.

## Log Levels

| Level | When | Environments |
|-------|------|--------------|
| `TRACE` | Rarely; checkstyle workaround only | local |
| `DEBUG` | Debugging only; not visible in prod | local, int, staging |
| `INFO` | State changes (new customer, process start) — not batch loops | all |
| `WARN` | Exceptional but non-fatal; client errors (4xx) max at WARN | all |
| `ERROR` | Process stopped with error; actively monitored; triggers alerts | all |
| `FATAL` | Never use | — |

**Goal: 0 ERROR entries in production at any time.**  
If an error appears regularly and "isn't a real problem" — it's the wrong level.

## HTTP Status → Log Level
| Status | Method | Level |
|--------|--------|-------|
| 2xx | GET | DEBUG |
| 2xx | PUT, POST, DELETE | INFO |
| 3xx | any | WARN |
| 4xx | any | WARN |
| 5xx | any | ERROR |

## Confidential Data — NEVER LOG
- Personal data: name, address, birth date, email, phone
- Identity documents: ID card, passport, SSN, tax ID
- Bank data: IBAN, account numbers, credit/debit card numbers
- Do NOT log full objects (esp. records — all fields are exposed via `toString()`)

```java
// BAD
LOGGER.debug("Loaded Customer: {}", customer);

// GOOD
LOGGER.debug("Loaded Customer with number: {}", customer.customerNumber());
```

### toString() protection
```java
// On sensitive records/classes:
@ToString(onlyExplicitlyIncluded = true)
// Add @ToString.Include only on safe fields
```
Use `ToStringMask` for masking sensitive fields in toString output.

## Forbidden Patterns

### System.out / stderr
```java
// FORBIDDEN
System.out.println(...);
e.printStackTrace();
```

### Catch-Log-Rethrow
```java
// FORBIDDEN — causes duplicate log entries
catch (IOException e) {
    LOGGER.error("...");
    throw new IllegalStateException(e); // also logged upstream
}
```

### Exception messages with personal data
Check exception `.getMessage()` contents before logging — personal data must not appear.

## Notifications
- Applications log to Graylog (backend) or Sentry (frontend) only
- Apps do NOT send log-triggered alerts directly (Slack, Email, Grafana) — configure those in the log management system

## Output format
When generating logging code, apply all rules above. Mark any confidential field exposure with `// VIOLATION: confidential data`.
