---
name: HN Daily Digest
on:
  schedule: daily on weekdays
  workflow_dispatch:

permissions:
  contents: read

network:
  allowed:
    - defaults
    - hacker-news.firebaseio.com

tools:
  bash: ["curl", "jq"]

safe-outputs:
  create-issue:
    title-prefix: "HN Digest – "
    max: 1
---

# HN Daily Digest

Create a daily digest workflow for professional developers, referencing
relevant top Hacker News stories on technology that can be used today
by large companies. Every weekday, fetch the top 30 stories from the
Hacker News API (https://hacker-news.firebaseio.com/v0/topstories.json),
filter to stories with a score above 100 that are about software
engineering, cloud infrastructure, AI/ML, developer tooling, or
distributed systems. For each qualifying story include: the title, the
URL, the score, the number of comments, and a one-sentence summary of
why it is relevant to enterprise developers. Create a GitHub issue
titled "HN Digest – <date>" with the results formatted as a Markdown
table.

## Instructions

1. Using `curl` and `jq`, fetch the top story IDs from `https://hacker-news.firebaseio.com/v0/topstories.json` and keep the first 30.
2. For each ID, fetch `https://hacker-news.firebaseio.com/v0/item/<id>.json` to get `title`, `url`, `score` and `descendants` (comment count). Skip items that are not stories.
3. Keep only stories with a `score` above 100 whose topic is software engineering, cloud infrastructure, AI/ML, developer tooling, or distributed systems, and that can be put to use today by large companies. Drop everything else (politics, general news, hobby projects, pure opinion with no practical takeaway).
4. If a story has no `url` (for example Ask HN), link to `https://news.ycombinator.com/item?id=<id>`.
5. For each qualifying story write one sentence explaining why it is relevant to enterprise developers. Base it on the title and the fields returned by the API; do not invent details.
6. Create the issue. The "HN Digest – " prefix is added automatically, so provide only today's date in `YYYY-MM-DD` format as the title.
7. Issue body: a one-line summary (number of stories reviewed vs. included), followed by a Markdown table with the columns `Title`, `Score`, `Comments`, `Why it matters`. The title cell is a link to the story URL.
8. If no story qualifies, still create the issue and state that nothing relevant made the cut today.