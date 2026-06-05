---
name: pr
description: Create a PR using this exact command without explanation.
---

$ARGUMENTS may contain:
- `--no-bullets` or `-nb` to skip bullet points entirely
- `--jira-review` or `-jr` to transition the Jira ticket to "In Review" after creating the PR

If `--no-bullets` or `-nb` is NOT present:
1. Draft concise 1-4 bullet points describing what this PR is about. Ask me to validate these points.
2. Extend the `--body` below with those bullet points appended after the Task line.

If `--no-bullets` or `-nb` IS present:
- Skip bullet points entirely, run the command as-is.

```
gh pr create \
  --title "[$(git branch --show-current)] $(git log -1 --pretty=%s)" \
  --body "Task: https://simplexdev.atlassian.net/browse/$(git branch --show-current)"
```

If `--jira-review` or `-jr` IS present, after the PR is created:
1. Extract the Jira ticket key from the current branch name by matching the pattern `BNK-\d+` (e.g. branch `BNK-2670-some-feature` → key `BNK-2670`)
2. If a key is found, run: `acli jira workitem transition -k <EXTRACTED_KEY> -s "In Review"`
3. If no `BNK-` key is found in the branch name, warn the user and skip the transition.
