# Tool Evaluation Framework

**Class of work**: Adopting-or-skipping a new external tool.

A 10-minute structured evaluation that turns a tool announcement or GitHub link into a verified authenticity report, a side-by-side comparison against the current stack, and an explicit adopt/hold/skip recommendation. Use this whenever a tool, library, or open-source project lands in chat and the user wants to know whether to act on it.

## Trigger phrases

- "Should we adopt this?" / "Is this useful for us?"
- "Look at this GitHub project" / "Eval this repo"
- A URL or product name + "评估一下" / "分析下对我们用处大不大"
- Inbound tool announcements, comparison requests, vendor pitches

## Required inputs

At least one of:

- GitHub / GitLab / Gitee URL
- Product name and homepage
- Direct text describing what the tool does

If none of these are present, ask once before skipping.

## Steps

### 1. Verify authenticity (mandatory before anything else)

Many inbound links are phishing, typo-squatting, or social-engineering bait. Validate before reading the README.

1. Hit the URL with `curl -sI -L -w "%{http_code}\n%{url_effective}"` (URL-encode non-ASCII characters).
2. For GitHub, also call `https://api.github.com/repos/{owner}/{repo}` and parse the JSON. Required fields: `stargazers_count`, `created_at`, `pushed_at`, `language`, `license`, `open_issues_count`, `default_branch`.
3. If 404 or API errors with "Not Found": **STOP**, mark as "phishing/typo-squat suspected", do not clone or recommend.
4. If real, compute the "6 red flags" score:

   | # | Red flag | Weight |
   |---|---|---|
   | 1 | Repo name has unusual characters (diacritics, oddly placed hyphens) | +2 |
   | 2 | Link arrived via video/WeChat/short-video, not GitHub Trending or HN | +2 |
   | 3 | README opens with grand narrative, no concrete technical detail | +1 |
   | 4 | Setup instructions are one-liner shell commands (`git clone && bash setup.sh`) | +2 |
   | 5 | High stars but commits are README-only changes | +1 |
   | 6 | Pain points in marketing copy mirror a current dev hot topic (free / local / AI / zero-cost) | +1 |

   Total ≥ 4 → "high suspicion, do not clone". 2-3 → "investigate further". ≤1 → proceed.

5. Compute the "5 hard indicators of real OSS":

   | # | Indicator | Required? |
   |---|---|---|
   | 1 | Repo accessible, non-empty | yes |
   | 2 | Created ≥ 6 months ago, sustained commit history | yes |
   | 3 | ≥ 3 independent contributors | yes |
   | 4 | Issues have real discussion with maintainer responses | recommended |
   | 5 | Test suite + License file present | recommended |

   Any "yes" not satisfied → note it; if #1 fails, abort.

### 2. Extract capabilities (≤ 5 minutes)

Use `web_fetch` on the repo README or product homepage. Pull:

- One-line product positioning
- 3-7 headline features
- Deployment options (Docker, npm, SaaS, etc.)
- License type — flag "Other" and look up the actual license file
- Maintenance signals (last commit, release cadence)

Quote README claims verbatim when they will appear in the comparison table. If the README is 30+ pages, stop reading after the feature list — do not summarise marketing fluff.

### 3. Compare against your current stack (the table that earns its keep)

Build a row-by-row matrix. Pull each capability category from the user's known stack (for OpenClaw: skills, sessions_spawn, memory-core, plugins.entries.*, worktree, etc.). Required columns:

| Capability | Candidate tool | Current stack winner | Notes |

Rules:

- The user does not want a feature dump — they want "is this an upgrade?"
- Mark any row where the candidate ties the current stack as "tie". Don't oversell differences.
- One row per user-validated use case (how many Agents, what channel, what scale), not per feature.

### 4. Adoption recommendation (the only thing the user actually reads)

End with exactly one of three stances, each with rationale and a minimum viable trial:

- **Adopt** — Use this. Replace current X with candidate Y. Trial: smallest action that proves the value.
- **Hold** — Don't integrate now. Set a check-in date and the trigger that would flip the call (e.g., "License clarifies to Apache 2.0", "Sustained 1k stars/month for 3 months").
- **Skip** — Don't touch. Justify why even a trial is wasted effort.

Never recommend "try it if you have time". Either there is a concrete trial with success criteria, or the call is Skip.

### 5. Anti-patterns to refuse

- Victim of marketing copy: do not echo "AI-powered", "next-generation", "10x faster" verbatim.
- Tool-pilling: never recommend adding more than one new dependency in a single evaluation.
- Privacy bypass: if the tool requires sending data to a public cloud, call this out and refuse to recommend it without an explicit self-hosted alternative.
- Aspirational demo: do not assume the demo experience matches the real product.

## Verification

The user should be able to read the final recommendation and answer three questions:

1. Did we actually verify the project is real, or did we just read the README?
2. Where does this beat what I already have, by how much, and is that worth the migration cost?
3. If adopt: what is the smallest experiment that proves or disproves the value, and what is the kill switch?

If any of the three is unanswerable, the report is not done. Reopen the step that was rushed and fix it.
