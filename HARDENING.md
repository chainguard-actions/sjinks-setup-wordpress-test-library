<!-- markdownlint-disable -->

# Hardening Report: sjinks--setup-wordpress-test-library/v2.1.4

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **sjinks--setup-wordpress-test-library/v2.1.4** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### hardcoded-credentials (severity: high)

The `db_password` input in action.yml has a hardcoded literal default value of `'wordpress'`. A non-expression literal password is assigned to a field whose name contains 'password'. While this is a well-known WordPress test password, it is still a hardcoded credential that could be inadvertently used in production environments. The value should be left empty (no default) and required to be explicitly provided by the caller.

Locations:

- `action.yml:22`

## Iteration Notes

### Iteration 1

**Fixes applied:** hardcoded-credentials

**Notes:**

Removed the hardcoded default password 'wordpress' from the db_password input in action.yml (line 22). Changed required from false to true so callers must explicitly provide the database password. This prevents the well-known WordPress test password from being inadvertently used in production environments.

