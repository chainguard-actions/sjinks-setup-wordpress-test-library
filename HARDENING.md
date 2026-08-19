<!-- markdownlint-disable -->

# Hardening Report: sjinks--setup-wordpress-test-library/v2.1.2

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **sjinks--setup-wordpress-test-library/v2.1.2** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A GitHub Actions expression is directly interpolated inside a `run:` shell command string. In the 'Check the diff' step, `${{ github.event.head_commit.committer.email }}` is embedded directly in the shell script: `if [ "${{ github.event.head_commit.committer.email }}" != "actions@github.com" ]; then`. A commit author who controls the committer email could inject shell metacharacters and execute arbitrary commands.

Locations:

- `.github/workflows/ci.yml:100`

### script-injection (severity: high)

Sub-rule (a): A GitHub Actions expression is directly interpolated inside a `run:` shell command string. In the 'Verify MariaDB connection' step, `${{ job.services.mysql.ports[3306] }}` is embedded directly in the shell script: `while ! mysqladmin ping -h 127.0.0.1 -P ${{ job.services.mysql.ports[3306] }} --silent; do`. The `job.*` context is YAML-template-substituted before the shell processes it, allowing injection of shell metacharacters.

Locations:

- `.github/workflows/ci-integration.yml:60`

### hardcoded-credentials (severity: high)

The `db_password` input in action.yml has a hardcoded literal default value of `'wordpress'`. This matches the pattern `password: <literal-value>` and constitutes a hardcoded credential embedded in the action definition.

Locations:

- `action.yml:21`

### hardcoded-credentials (severity: high)

The ci-integration.yml workflow hardcodes literal database passwords in the MySQL service container environment: `MYSQL_ROOT_PASSWORD: wordpress` (line 25) and `MYSQL_PASSWORD: wordpress` (line 27). These are plaintext credentials embedded directly in the workflow file.

Locations:

- `.github/workflows/ci-integration.yml:25`
- `.github/workflows/ci-integration.yml:27`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, hardcoded-credentials

**Notes:**

Fixed 4 findings across 3 files:
1. ci.yml: Moved `${{ github.event.head_commit.committer.email }}` out of the `run:` shell string into an `env:` block (`COMMITTER_EMAIL`) and referenced it as `$COMMITTER_EMAIL` in the shell script.
2. ci-integration.yml (script-injection): Moved `${{ job.services.mysql.ports[3306] }}` out of the `run:` shell string into an `env:` block (`MYSQL_PORT`) and referenced it as `"$MYSQL_PORT"` in the shell script.
3. ci-integration.yml (hardcoded-credentials): Replaced hardcoded literal passwords `wordpress` for `MYSQL_ROOT_PASSWORD` and `MYSQL_PASSWORD` with secret references (`${{ secrets.MYSQL_ROOT_PASSWORD || 'test_root_password' }}` and `${{ secrets.MYSQL_PASSWORD || 'test_db_password' }}`).
4. action.yml (hardcoded-credentials): Removed the hardcoded default value `'wordpress'` from the `db_password` input definition so no credential is embedded in the action definition.

