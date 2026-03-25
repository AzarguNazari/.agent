---
name: openapi-spec
description: >-
  OpenAPI-first REST API design, spec location, review process, and Redocly
  lint/build for public APIs.
---

## Trigger
Use when designing, writing, or reviewing REST API specifications or new endpoints.

## Design Rules
- No API versioning (`/v1/`, `/v2/`) — all clients are known; breaking changes are coordinated directly
- Prefer backward-compatible changes: deprecate fields instead of removing; add `deprecated: true` before removal
- Breaking changes and deprecations MUST be communicated to all consumers via dedicated API Slack channels
- Field removal or rename: mark deprecated first, coordinate removal separately

## Why Spec-First
- Enables frontend/backend parallel development (frontend mocks from spec)
- Aligns teams on contracts before code is written
- Enables E2E testers to validate against intent
- Reduces coordination overhead for multi-team API consumers
