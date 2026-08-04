<!-- markdownlint-disable -->

# Hardening Report: dailydotdev--action-devcard/2.3.1

> This file was generated automatically by the hardening agent.

**Policy SHA:** `d636be7e43ef829af6e853da6b3c7566db9f72fe`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **dailydotdev--action-devcard/2.3.1** was hardened automatically. 3 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### script-injection (severity: high)

Sub-rule (a): A ${{ ... }} expression is directly interpolated inside a run: shell command. In the 'Bump version' step, `npm version ${{ github.event.inputs.version }} --no-git-tag-version` injects the workflow_dispatch input directly into the shell command string before the shell ever sees it. Even though the input is a choice type, the YAML template substitution happens before any shell quoting, making this a script-injection risk. The value should be passed via an env: variable and referenced as a quoted shell variable instead.

Locations:

- `.github/workflows/release.yml:56`

### unpinned-uses (severity: high)

Multiple workflow files reference external actions using mutable tag or version refs instead of immutable 40-character commit SHA pins. Unpinned references are vulnerable to supply-chain attacks if the upstream tag is moved or the repository is compromised. Failing references:

.github/workflows/code-ql.yml:
  - actions/checkout@v4
  - github/codeql-action/init@v2
  - github/codeql-action/analyze@v2

.github/workflows/continuous-integration.yml:
  - actions/checkout@v4
  - actions/setup-node@v3.8.1
  - actions/cache@v3.3.2

.github/workflows/dependabot-automerge.yaml:
  - dependabot/fetch-metadata@v1.6.0

.github/workflows/labels.yml:
  - actions/checkout@v4
  - crazy-max/ghaction-github-labeler@v5.0.0

.github/workflows/release.yml:
  - actions/checkout@v4
  - chainguard-dev/actions/setup-gitsign@main
  - actions/setup-node@v3.8.1
  - actions/cache@v3.3.2

Locations:

- `.github/workflows/code-ql.yml:18`
- `.github/workflows/continuous-integration.yml:18`
- `.github/workflows/dependabot-automerge.yaml:20`
- `.github/workflows/labels.yml:16`
- `.github/workflows/release.yml:26`

### missing-permissions (severity: medium)

Three workflow files have no top-level `permissions:` key and no job-level `permissions:` key on any of their jobs. Without explicit permissions, workflows inherit the repository's default token permissions, which may be overly broad (e.g. write access to contents). Each file should declare minimal required permissions.

- .github/workflows/code-ql.yml: no permissions declared at top-level or job level
- .github/workflows/continuous-integration.yml: no permissions declared at top-level or job level
- .github/workflows/labels.yml: no permissions declared at top-level or job level

Locations:

- `.github/workflows/code-ql.yml:1`
- `.github/workflows/continuous-integration.yml:1`
- `.github/workflows/labels.yml:1`

## Iteration Notes

### Iteration 1

**Fixes applied:** script-injection, unpinned-uses, missing-permissions

**Notes:**

Fixed all three finding types across 5 workflow files:

1. script-injection (release.yml): Moved `${{ github.event.inputs.version }}` into an env: block as INPUT_VERSION and referenced it as "$INPUT_VERSION" in the shell script.

2. unpinned-uses: Pinned all 8 action references to full 40-character commit SHAs across code-ql.yml, continuous-integration.yml, dependabot-automerge.yaml, labels.yml, and release.yml. Original tags preserved as inline comments.

3. missing-permissions: Added minimal top-level permissions blocks to code-ql.yml (actions: read, contents: read, security-events: write), continuous-integration.yml (contents: read), and labels.yml (contents: read, issues: write).

