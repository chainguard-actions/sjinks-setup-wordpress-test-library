<!-- markdownlint-disable -->

# Hardening Report: sjinks--setup-wordpress-test-library/v2.1.5

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **sjinks--setup-wordpress-test-library/v2.1.5** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### hardcoded-credentials (severity: high)

The `db_password` input in action.yml has a hardcoded literal default value of `'wordpress'`. This is a well-known default credential that could be used insecurely if callers rely on the default. The pattern `db_password: ... default: 'wordpress'` matches the hardcoded-credentials check (a name containing 'password' assigned a non-expression literal value).

Locations:

- `action.yml:22`

## Iteration Notes

### Iteration 1

**Fixes applied:** hardcoded-credentials

**Notes:**

Removed the hardcoded default value 'wordpress' from the db_password input in action.yml (line 22). Changed the input from 'required: false' with 'default: wordpress' to 'required: true' with no default. This forces callers to explicitly provide a database password rather than relying on a well-known default credential.

