<!-- markdownlint-disable -->

# Hardening Report: sjinks--setup-wordpress-test-library/v3.0.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **sjinks--setup-wordpress-test-library/v3.0.0** was hardened automatically. 2 finding(s) were identified and resolved across 2 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A GitHub Actions expression is directly interpolated inside a `run:` shell command. The step 'Verify MariaDB connection' uses `${{ job.services.mysql.ports[3306] }}` directly in the shell command string: `while ! mysqladmin ping -h 127.0.0.1 -P ${{ job.services.mysql.ports[3306] }} --silent; do`. Any `${{ ... }}` expression inside a `run:` block undergoes YAML template substitution before the shell sees it, bypassing shell quoting. The value should be passed via an `env:` variable and referenced as a double-quoted shell variable instead.

Locations:

- `.github/workflows/ci-integration.yml:72`

### hardcoded-credentials (severity: high)

Hardcoded literal passwords are set in the service container environment. `MYSQL_ROOT_PASSWORD: wordpress` and `MYSQL_PASSWORD: wordpress` are plain-text literal values, not GitHub Actions secret expressions (`${{ secrets.* }}`). These credentials are exposed in the workflow file and should be replaced with secret references.

Locations:

- `.github/workflows/ci-integration.yml:25`
- `.github/workflows/ci-integration.yml:28`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, hardcoded-credentials

**Notes:**

Fixed two findings in .github/workflows/ci-integration.yml: (1) script-injection: moved `${{ job.services.mysql.ports[3306] }}` from the `run:` shell string into an `env:` block as `MYSQL_PORT`, referenced as `"$MYSQL_PORT"` in the shell command; (2) hardcoded-credentials: replaced plain-text `wordpress` values for `MYSQL_ROOT_PASSWORD` and `MYSQL_PASSWORD` with `${{ secrets.MYSQL_ROOT_PASSWORD }}` and `${{ secrets.MYSQL_PASSWORD }}` respectively.

### Iteration 2

**Fixes applied:** hardcoded-credentials

**Notes:**

Removed the hardcoded default value 'wordpress' from the db_password input in action.yml (line 24). Changed the field from `required: false` with `default: 'wordpress'` to `required: true` with no default. This forces users to explicitly provide a database password rather than relying on a hardcoded value that could be inadvertently used in production deployments.

