<!-- markdownlint-disable -->

# Hardening Report: sjinks--setup-wordpress-test-library/v2.1.3

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **sjinks--setup-wordpress-test-library/v2.1.3** was hardened automatically. 3 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A ${{ }} expression is directly interpolated inside a run: shell command. In ci-integration.yml, the 'Verify MariaDB connection' step uses `${{ job.services.mysql.ports[3306] }}` directly in a shell command: `while ! mysqladmin ping -h 127.0.0.1 -P ${{ job.services.mysql.ports[3306] }} --silent; do`. Any ${{ ... }} expression inside a run: block is a script-injection risk as the value is substituted by the YAML template engine before the shell ever sees it.

Locations:

- `.github/workflows/ci-integration.yml:52`

### script-injection (severity: high)

Sub-rule (a): A ${{ }} expression is directly interpolated inside a run: shell command. In ci.yml, the 'Check the diff' step uses `${{ github.event.head_commit.committer.email }}` directly in a shell if-statement: `if [ "${{ github.event.head_commit.committer.email }}" != "actions@github.com" ]; then`. The github.event.head_commit.committer.email value is attacker-controllable via commit metadata and is substituted into the shell command before execution, enabling script injection.

Locations:

- `.github/workflows/ci.yml:89`

### hardcoded-credentials (severity: high)

Literal hardcoded passwords are assigned to password-named environment variables in the MySQL service container configuration. `MYSQL_ROOT_PASSWORD: wordpress` and `MYSQL_PASSWORD: wordpress` are plaintext credential values embedded directly in the workflow file, matching the pattern `(?i)(password)\s*[=:]\s*[A-Za-z0-9][A-Za-z0-9+/=_\-]{7,}`. These should be stored as GitHub Actions secrets and referenced via `${{ secrets.* }}`.

Locations:

- `.github/workflows/ci-integration.yml:24`
- `.github/workflows/ci-integration.yml:27`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, hardcoded-credentials

**Notes:**

Fixed three findings across two workflow files:
1. ci-integration.yml: Moved `${{ job.services.mysql.ports[3306] }}` out of the run: shell command into an env: block (MYSQL_PORT), eliminating script injection risk.
2. ci-integration.yml: Replaced hardcoded plaintext passwords `MYSQL_ROOT_PASSWORD: wordpress` and `MYSQL_PASSWORD: wordpress` with `${{ secrets.MYSQL_ROOT_PASSWORD }}` and `${{ secrets.MYSQL_PASSWORD }}` respectively.
3. ci.yml: Moved `${{ github.event.head_commit.committer.email }}` out of the run: shell command into an env: block (COMMITTER_EMAIL), eliminating script injection risk from attacker-controllable commit metadata.

### Iteration 2

**Fixes applied:** hardcoded-credentials

**Notes:**

Removed the hardcoded default password 'wordpress' from the db_password input in action.yml (line 22). Changed the input from 'required: false' with 'default: wordpress' to 'required: true' with no default. This forces callers to explicitly supply a database password rather than silently inheriting a well-known credential.

