<!-- markdownlint-disable -->

# Hardening Report: sjinks--setup-wordpress-test-library/v2.1.4

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **sjinks--setup-wordpress-test-library/v2.1.4** was hardened automatically. 4 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A ${{ }} expression is directly interpolated inside a run: shell command. In ci-integration.yml, `${{ job.services.mysql.ports[3306] }}` is embedded directly in a shell command string: `while ! mysqladmin ping -h 127.0.0.1 -P ${{ job.services.mysql.ports[3306] }} --silent; do`. Although job.services is not attacker-controlled, any ${{ }} expression inside a run: block is a script-injection finding per the check rules, as the value flows through YAML template substitution before the shell ever sees it.

Locations:

- `.github/workflows/ci-integration.yml:57`

### script-injection (severity: high)

Sub-rule (a): A ${{ }} expression is directly interpolated inside a run: shell command. In ci.yml, `${{ github.event.head_commit.committer.email }}` is embedded directly in a shell command string: `if [ "${{ github.event.head_commit.committer.email }}" != "actions@github.com" ]; then`. The `github.event.head_commit.committer.email` value is attacker-controllable (e.g. via a crafted commit) and is injected directly into the shell before quoting can protect it.

Locations:

- `.github/workflows/ci.yml:99`

### hardcoded-credentials (severity: high)

Literal hardcoded password found in action.yml: the `db_password` input has a default value of `'wordpress'`, a plaintext literal password. This matches the pattern `password: wordpress`.

Locations:

- `action.yml:21`

### hardcoded-credentials (severity: high)

Literal hardcoded passwords found in ci-integration.yml: the MySQL service container is configured with `MYSQL_ROOT_PASSWORD: wordpress` and `MYSQL_PASSWORD: wordpress` as plaintext literal values in the workflow env block.

Locations:

- `.github/workflows/ci-integration.yml:22`
- `.github/workflows/ci-integration.yml:25`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, hardcoded-credentials

**Notes:**

Fixed all four findings: (1) ci-integration.yml script-injection: moved `${{ job.services.mysql.ports[3306] }}` into an env block as MYSQL_PORT and referenced it as "$MYSQL_PORT" in the shell; (2) ci.yml script-injection: moved `${{ github.event.head_commit.committer.email }}` into an env block as COMMITTER_EMAIL and referenced it as "$COMMITTER_EMAIL" in the shell; (3) action.yml hardcoded-credentials: changed db_password default from 'wordpress' to '' (empty string); (4) ci-integration.yml hardcoded-credentials: replaced MYSQL_ROOT_PASSWORD and MYSQL_PASSWORD plaintext values with ${{ secrets.MYSQL_ROOT_PASSWORD }} and ${{ secrets.MYSQL_PASSWORD }} respectively.

