# Tool Evaluation Framework

A reusable evaluation procedure for deciding whether to adopt a new tool, library, or open-source project.

Built for AI agents and developers who receive frequent inbound tool recommendations (GitHub links, video tutorials, vendor pitches) and need a fast, structured way to decide: **Adopt / Hold / Skip**.

## What it solves

When a new tool lands in your inbox, the typical reaction is to clone it and try it. That's how phishing repos get executed and how tool-pilling begins. This framework forces a 10-minute structured pass before any installation:

1. **Verify authenticity** — curl + GitHub API + 6 red flags + 5 hard indicators
2. **Extract capabilities** — web_fetch the README, extract positioning + features + license
3. **Compare to current stack** — row-by-row matrix against what you already use
4. **Issue verdict** — exactly one of: Adopt / Hold / Skip, each with rationale + minimum viable trial

End with three verification questions the user must be able to answer.

## Who it's for

- AI agents whose users keep asking "should we adopt this?"
- Engineering teams evaluating new dependencies
- Anyone tired of cloning random repos from social media

## Usage

This is a **skill** for OpenClaw or compatible AgentSkills frameworks. Drop the `SKILL.md` into your `~/.openclaw/skills/` (or equivalent) directory and invoke it when the trigger phrases appear in conversation.

### Trigger phrases

- "Should we adopt this?" / "Is this useful for us?"
- "Look at this GitHub project" / "Eval this repo"
- A URL or product name + "评估一下" / "分析下对我们用处大不大"

### Required inputs

At least one of:

- GitHub / GitLab / Gitee URL
- Product name + homepage
- Direct description of what the tool does

## The 6 Red Flags (authenticity check)

| # | Red flag | Weight |
|---|---|---|
| 1 | Repo name has unusual characters (diacritics, oddly placed hyphens) | +2 |
| 2 | Link arrived via video / WeChat / short-video, not GitHub Trending or HN | +2 |
| 3 | README opens with grand narrative, no concrete technical detail | +1 |
| 4 | Setup instructions are one-liner shell commands (`git clone && bash setup.sh`) | +2 |
| 5 | High stars but commits are README-only changes | +1 |
| 6 | Pain points in marketing copy mirror a current dev hot topic | +1 |

**Total ≥ 4** → high suspicion, do not clone. **2-3** → investigate further. **≤1** → proceed.

## The 5 Hard Indicators of genuine OSS

| # | Indicator | Required? |
|---|---|---|
| 1 | Repo accessible, non-empty | yes |
| 2 | Created ≥ 6 months ago, sustained commit history | yes |
| 3 | ≥ 3 independent contributors | yes |
| 4 | Issues have real discussion with maintainer responses | recommended |
| 5 | Test suite + License file present | recommended |

## The Three Verdicts

- **Adopt** — Replace current X with candidate Y. Trial: smallest action that proves the value.
- **Hold** — Don't integrate now. Set a check-in date + the trigger that flips the call.
- **Skip** — Don't touch. Justify why even a trial is wasted effort.

Never recommend "try it if you have time". Either there is a concrete trial with success criteria, or the call is Skip.

## Provenance

Built 2026-09-06 from two real evaluations done in one day:

1. `paciño/atlas` (a phishing / typo-squat candidate) — correctly flagged as 404 + "do not clone" before any installation.
2. `lobehub/lobehub` (a real 82k-star project) — passed all 5 hard indicators, received a **Hold** verdict (interesting long-term, not urgent for current stack).

The framework is the difference between those two outcomes being **snap judgments** and **defensible recommendations**.

## License

MIT — see [LICENSE](./LICENSE).
