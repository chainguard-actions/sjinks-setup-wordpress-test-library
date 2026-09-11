<!-- markdownlint-disable -->

# Hardening Report: sjinks--setup-wordpress-test-library/v2.1.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **sjinks--setup-wordpress-test-library/v2.1.2** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### hardcoded-credentials (severity: high)

The action.yml file contains a hardcoded literal password as the default value for the `db_password` input: `default: 'wordpress'`. A literal credential value is assigned to a field whose name contains 'password'. Even though this is a default for a test database, hardcoding credentials in the action definition is a security risk — callers may unknowingly use the insecure default, and it establishes a pattern of embedding credentials in source. The default should be empty and callers should be required to supply a credential via a GitHub Actions secret.

Locations:

- `action.yml:24`

## Iteration Notes

### Iteration 1

**Fixes applied:** hardcoded-credentials

**Notes:**

Removed the hardcoded default password 'wordpress' from the db_password input in action.yml (line 24). Changed the field from 'required: false' with 'default: wordpress' to 'required: true' with no default, forcing callers to supply a credential explicitly (e.g., via a GitHub Actions secret).

