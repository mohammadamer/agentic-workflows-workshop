---
description: Weekday digest of all open issues and pull requests, grouped by label.
on:
  schedule: daily on weekdays
  workflow_dispatch:

permissions:
  contents: read
  issues: read
  pull-requests: read

# Ensure the environment has access to required tokens
env:
  COPILOT_GITHUB_TOKEN: ${{ secrets.COPILOT_GITHUB_TOKEN }}

tools:
  github:
    toolsets: [default]

network: defaults

safe-outputs:
  create-issue:
    title-prefix: "Daily Digest – "
    max: 1
---

# Daily Digest

Create a single GitHub issue summarising all open issues and open pull requests in this repository.

## Instructions

1. Use the GitHub tools to list every open issue and every open pull request in `${{ github.repository }}` (paginate until complete). Exclude previous digest issues (titles starting with "Daily Digest").
2. Group items by label. Items with several labels appear under each of their labels. Items with no label go under "Unlabeled". Order groups alphabetically, with "Unlabeled" last.
3. For each item include: type (issue or PR), number linked as `#N`, title, author (`@login`), and how long it has been open (e.g. "3 days", "5 hours"), computed from its creation time to now.
4. Create the issue with title `Daily Digest – <date>`. The "Daily Digest – " prefix is added automatically, so provide only today's date in `YYYY-MM-DD` format as the title.
5. Issue body structure:
    - A summary line with the total count of open items, split into issues and pull requests.
    - One `###` section per label with its item count, listing items as a table or bullet list with the fields above.
6. If nothing is open, still create the issue stating that there is nothing open.