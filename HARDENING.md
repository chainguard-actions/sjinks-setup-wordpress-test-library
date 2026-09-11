<!-- markdownlint-disable -->

# Hardening Report: sjinks--setup-wordpress-test-library/v2.1.3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **sjinks--setup-wordpress-test-library/v2.1.3** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### hardcoded-credentials (severity: high)

The `db_password` input in action.yml has a hardcoded literal default value of `'wordpress'`. A non-expression literal password is assigned to a field named `db_password` (line 21), with `default: 'wordpress'` on line 24. While this is a default intended for test/local use, shipping a hardcoded password as the default for a credential field is a security risk — callers may inadvertently use it in production environments without overriding it.

Locations:

- `action.yml:24`

## Iteration Notes

### Iteration 1

**Fixes applied:** hardcoded-credentials

**Notes:**

Removed the hardcoded default password 'wordpress' from the db_password input in action.yml (line 24). Changed the field from 'required: false' with 'default: wordpress' to 'required: true' with no default. This forces callers to explicitly provide a database password, preventing accidental use of the hardcoded credential in production environments.

