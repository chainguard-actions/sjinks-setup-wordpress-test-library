<!-- markdownlint-disable -->

# Hardening Report: sjinks--setup-wordpress-test-library/v2.1.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `1`

Action **sjinks--setup-wordpress-test-library/v2.1.2** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### hardcoded-credentials (severity: high)

The action.yml file defines a `db_password` input with a hardcoded literal default value of `'wordpress'`. This matches the pattern for hardcoded credentials (password field with a non-expression literal value). Callers who do not override this input will use the weak default password, which could lead to insecure database configurations in CI environments.

Locations:

- `action.yml:22`

## Iteration Notes

### Iteration 1

**Fixes applied:** hardcoded-credentials

**Notes:**

Removed the hardcoded default value 'wordpress' from the db_password input in action.yml (line 22). Changed the input from `required: false` with `default: 'wordpress'` to `required: true` with no default. This forces callers to explicitly supply a database password rather than silently using a weak hardcoded credential.

