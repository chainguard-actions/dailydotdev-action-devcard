<!-- markdownlint-disable -->

# Hardening Report: dailydotdev--action-devcard/3.2.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **dailydotdev--action-devcard/3.2.1** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### unpinned-uses (severity: high)

Multiple workflow files reference actions using mutable tags or branch names instead of pinned 40-character SHA commits, making them vulnerable to supply-chain attacks if the referenced tag is moved or the action is compromised.

.github/workflows/code-ql.yml: actions/checkout@v4, github/codeql-action/init@v3, github/codeql-action/analyze@v3
.github/workflows/continuous-integration.yml: actions/checkout@v4, actions/setup-node@v4.1.0, actions/cache@v4.2.2
.github/workflows/dependabot-automerge.yaml: dependabot/fetch-metadata@v2.2.0
.github/workflows/labels.yml: actions/checkout@v4, crazy-max/ghaction-github-labeler@v5.1.0
.github/workflows/release.yml: actions/checkout@v4, chainguard-dev/actions/setup-gitsign@main, actions/setup-node@v4.1.0, pnpm/action-setup@v4.0.0, actions/cache@v4.2.2

Locations:

- `.github/workflows/code-ql.yml:18`
- `.github/workflows/code-ql.yml:21`
- `.github/workflows/code-ql.yml:26`
- `.github/workflows/continuous-integration.yml:18`
- `.github/workflows/continuous-integration.yml:21`
- `.github/workflows/continuous-integration.yml:25`
- `.github/workflows/dependabot-automerge.yaml:17`
- `.github/workflows/labels.yml:15`
- `.github/workflows/labels.yml:18`
- `.github/workflows/release.yml:24`
- `.github/workflows/release.yml:26`
- `.github/workflows/release.yml:29`
- `.github/workflows/release.yml:31`
- `.github/workflows/release.yml:35`

### script-injection (severity: high)

Sub-rule (a): The 'Bump version' step in release.yml directly interpolates the GitHub Actions expression `${{ github.event.inputs.version }}` inside a `run:` shell command. Although this input is constrained to a `choice` type in the workflow_dispatch definition, the expression is still expanded by the Actions template engine before the shell sees it, making it a script-injection risk. The offending line is: `npm version ${{ github.event.inputs.version }} --no-git-tag-version`

Locations:

- `.github/workflows/release.yml:57`

### missing-permissions (severity: medium)

Three workflow files have no top-level `permissions:` key and no job-level `permissions:` key on any of their jobs. Without explicit permissions, workflows inherit the repository's default token permissions (which may be broad). Affected files: code-ql.yml (job: analyze), continuous-integration.yml (jobs: lint-build-test, dependabot), labels.yml (job: labeler).

Locations:

- `.github/workflows/code-ql.yml:1`
- `.github/workflows/continuous-integration.yml:1`
- `.github/workflows/labels.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** unpinned-uses, script-injection, missing-permissions

**Notes:**

Fixed all findings across 5 workflow files:

1. unpinned-uses: Pinned all 14 action references to full 40-char SHAs with original tag as comment. Actions pinned: actions/checkout@v4, github/codeql-action/init@v3, github/codeql-action/analyze@v3, actions/setup-node@v4.1.0, actions/cache@v4.2.2, dependabot/fetch-metadata@v2.2.0, crazy-max/ghaction-github-labeler@v5.1.0, chainguard-dev/actions/setup-gitsign@main, pnpm/action-setup@v4.0.0.

2. script-injection: In release.yml 'Bump version' step, moved `${{ github.event.inputs.version }}` out of the run: shell into an env: block as INPUT_VERSION, then referenced as "$INPUT_VERSION" in the shell script.

3. missing-permissions: Added top-level permissions blocks to code-ql.yml (actions:read, contents:read, security-events:write), continuous-integration.yml (contents:read), and labels.yml (contents:read, issues:write). The dependabot-automerge.yaml already had permissions defined.

