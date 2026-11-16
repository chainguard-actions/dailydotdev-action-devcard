<!-- markdownlint-disable -->

# Hardening Report: dailydotdev--action-devcard/3.1.0

> This file was generated automatically by the hardening agent.

**Policy SHA:** `374911c4be584df24e6da57af2d990570ef0f859`

**Test Policy SHA:** `843adf9e4b8f85d0c08b27b9d0b09dd094b54702`

**Harden Agent Version:** `2`

Action **dailydotdev--action-devcard/3.1.0** was hardened automatically. 1 finding(s) were identified and resolved across 1 iteration(s).

## Findings Fixed

### prior-fix (severity: high)

Carried forward from this version's previous harden, which fixed it; fix it again unless the code no longer has the problem. The previous harden changed action.yml without recording a finding for it, and this analysis reported nothing there. Re-apply its security changes unless the code no longer needs them. The previous patch:
```diff
diff --git a/action.yml b/action.yml
index 38ed43a..6662f85 100644
--- a/action.yml
+++ b/action.yml
@@ -8,47 +8,39 @@ inputs:
   user_id:
     description: 'Your daily.dev user id'
     required: true
-
   type:
     description: 'Configure orientation for devcard. Must be either "default" or "wide"'
     default: default
     required: false
-
   token:
     description: GitHub Token used to commit the devcard
     default: ${{ github.token }}
     required: false
-
   commit_branch:
     description: Branch used to commit the devcard
     default: ""
     required: false
-
   commit_message:
     description: Commit message
     default: Update ${filename}
     required: false
-
   commit_filename:
     description: Filename to save devcard to
     default: devcard.png
     required: false
-
   committer_email:
     description: Committer email
     default: 41898282+github-actions[bot]@users.noreply.github.com
     required: false
-
   committer_name:
     description: Committer name
     default: github-actions[bot]
     required: false
-
   dryrun:
     description: 'If true, no changes will be made to the repo'
     default: false
     required: false
-
 runs:
   using: 'node20'
   main: "dist/index.js"

```

Locations:

- `action.yml`

## Iteration Notes

### Iteration 1

**Notes:**

I reviewed the 'prior-fix' finding and made no changes. The earlier patch to action.yml only deleted blank lines between the input definitions, so there was no security change to re-apply. The current action.yml is a plain node20 action (main: dist/index.js) with no uses:, run: steps, permissions or expressions in shell, so it has no security problem to fix.

