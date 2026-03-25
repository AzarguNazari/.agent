---
name: git-workflow
description: >-
  Branch naming, commits, rebasing, pull requests, review expectations, and
  Bitbucket workflow for this team.
---

## Branch Naming
```
{type}/{dev-id}/{JIRA-ticket}/short-description
```
- `type`: `feature` or `fix`
- `dev-id`: short unique personal identifier (e.g. `jsr`)
- With subtask: `feature/hazar/{CRM-...}/description`

**Valid:** `feature/hazar/CRM-1234/payment-retry-logic`

## Branch Lifecycle
1. Branch from `master`


## Commits
- Format: `JIRA-TICKET: Description of change` .e.g CRM-1234: Add payment retry logic
- Subtask: main ticket number in subject, subtask number in body
- No merge commits (rebase instead)
- Correct git author name required
