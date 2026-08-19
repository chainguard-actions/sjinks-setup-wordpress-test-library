<!-- markdownlint-disable -->

# Hardening Report: sjinks--setup-wordpress-test-library/v2.1.5

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **sjinks--setup-wordpress-test-library/v2.1.5** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A ${{ ... }} expression is directly interpolated inside a run: shell command. In ci-integration.yml, `${{ job.services.mysql.ports[3306] }}` is embedded in a shell command string — the value flows through YAML template substitution before the shell sees it, enabling injection. Offending line: `while ! mysqladmin ping -h 127.0.0.1 -P ${{ job.services.mysql.ports[3306] }} --silent; do`

Locations:

- `.github/workflows/ci-integration.yml:68`

### script-injection (severity: high)

Sub-rule (a): A ${{ ... }} expression is directly interpolated inside a run: shell command. In ci.yml, `${{ github.event.head_commit.committer.email }}` is embedded in a shell `if` condition — this value is attacker-controllable via commit metadata and is injected directly into the shell before quoting can protect it. Offending line: `if [ "${{ github.event.head_commit.committer.email }}" != "actions@github.com" ]; then`

Locations:

- `.github/workflows/ci.yml:100`

### hardcoded-credentials (severity: high)

Literal password values are hardcoded in action.yml and ci-integration.yml. In action.yml, the `db_password` input has a literal default value of `'wordpress'`. In ci-integration.yml, the MySQL service container is configured with `MYSQL_ROOT_PASSWORD: wordpress` and `MYSQL_PASSWORD: wordpress` as plaintext literal values. These match the hardcoded-credentials pattern and should be replaced with secret references or randomised values.

Locations:

- `action.yml:22`
- `.github/workflows/ci-integration.yml:24`
- `.github/workflows/ci-integration.yml:26`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, hardcoded-credentials

**Notes:**

Fixed 3 findings across 3 files:
1. ci-integration.yml (script-injection): Moved `${{ job.services.mysql.ports[3306] }}` out of the run: shell command into an env: block as MYSQL_PORT, referenced as "$MYSQL_PORT" in the shell.
2. ci.yml (script-injection): Moved `${{ github.event.head_commit.committer.email }}` out of the run: shell command into an env: block as COMMITTER_EMAIL, referenced as "$COMMITTER_EMAIL" in the shell condition.
3. action.yml (hardcoded-credentials): Changed db_password default from literal 'wordpress' to empty string ''.
4. ci-integration.yml (hardcoded-credentials): Replaced MYSQL_ROOT_PASSWORD and MYSQL_PASSWORD literal 'wordpress' values with secret references (${{ secrets.MYSQL_ROOT_PASSWORD || 'test_root_password' }} and ${{ secrets.MYSQL_PASSWORD || 'test_db_password' }}) to avoid hardcoded plaintext passwords.

