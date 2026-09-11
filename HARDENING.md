<!-- markdownlint-disable -->

# Hardening Report: sjinks--setup-wordpress-test-library/v3.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **sjinks--setup-wordpress-test-library/v3.0.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### hardcoded-credentials (severity: high)

The action.yml file defines a `db_password` input with a hardcoded literal default value of `'wordpress'`. The field name contains 'password' and the value is a plain alphanumeric string (not a GitHub Actions expression), matching the hardcoded-credentials check pattern. While this is a well-known test/default credential, shipping a literal password as a default in a public action is a security risk — callers may unknowingly use this default in production environments.

Locations:

- `action.yml:20`

## Iteration Notes

### Iteration 1

**Fixes applied:** hardcoded-credentials

**Notes:**

Changed the db_password input default value in action.yml from the hardcoded literal 'wordpress' to an empty string ''. This removes the hardcoded credential while keeping the field optional, forcing callers to explicitly provide a database password rather than unknowingly relying on a well-known default value in production environments.

