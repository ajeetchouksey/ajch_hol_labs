# ajch_hol_labs

This repository is the canonical content source for Aarya's HOL Labs — guided, hands-on lab walkthroughs (prereqs → numbered steps with code → validation checklist → cleanup) that turn platform skills into practiced ability. Every lab is cross-linked to a real skillup exam domain, blog post, and/or use case wherever a genuine relationship exists. The platform consumes this repository via a pinned Git SHA through `content-manifest.json` in the main app repo (`ajeetchouksey/ajch_platform`), so content is versioned and reviewable before it goes live — the same model already used for `ajch_aaryaai_blogs`, `ajch_skillup`, and `ajch_ai_usecases`.

## What this repo contains

- `content/hol-labs/index.json` — catalog index: domains, per-lab summaries (title, difficulty, cost tier, flattened cross-link ids), and `featuredIds` — powers the `/hol-labs` platform page
- `content/hol-labs/labs/*.json` — one file per published lab, the full guided walkthrough — this is what `/hol-labs/{id}` renders
- `scripts/validate-content.mjs` — schema validator (canonical copy, synced from `ajch_platform`) — includes a `validateHolLab()` branch checking the full lab-JSON schema (id, schema, problemStatement, approachRationale, steps with required `whyItMatters`, costEstimate, etc.)
- `.github/workflows/validate-content.yml` — automated schema validation on PR/push

## Every lab teaches "why," not just "how"

Two fields exist specifically so a lab builds judgment instead of being a generic click-through tutorial: `problemStatement`/`approachRationale` (the real problem this solves and why this tool over a named alternative) and each step's `whyItMatters` (the underlying trade-off, not just the click). `conceptChecks` are short reflection questions after key steps that test applied reasoning, mirroring the platform's Principal Mentor Socratic teaching pattern. Every lab also states a `costEstimate` upfront — the platform's labs default to free/low-cost Azure/GitHub tiers, and any unavoidable paid resource is flagged before the first step, with a matching `cleanup` teardown for everything billable.

## Publishing model

This vertical has a dedicated content-authoring pipeline, matching the pattern used by Use Cases (Usecase Lead → Usecase Writer → AppSec Engineer → Usecase Publisher): **HOL Lab Lead → HOL Lab Writer → AppSec Engineer (Security Gate) → HOL Lab Publisher**, defined in `.claude/agents/`. The shared pipeline contract lives in `.claude/skills/vertical-pipeline/SKILL.md`.

**Content files are never written directly** — any addition or edit to `content/hol-labs/` should go through HOL Lab Lead as the entry point; see `CLAUDE.md` at this repo's root.

1. Ask HOL Lab Lead to draft one or more labs (it delegates research + drafting → HOL Lab Writer, validation → AppSec Engineer, and the actual file write + `index.json` update → HOL Lab Publisher).
2. Validate locally (below) — the agent pipeline runs this automatically as part of the Security Gate, but it's also safe to re-run by hand.
3. Open a PR, get it reviewed, merge to `main`.
4. From `ajch_platform`, promote the new SHA into `content-manifest.json` (via `node scripts/sync-vertical-repo.mjs hol-labs ajeetchouksey/ajch_hol_labs <sha>`).

Agent definitions in `.claude/agents/` and the pipeline skill in `.claude/skills/` are kept in sync with `ajch_platform`'s canonical copies via `node scripts/sync-vertical-agents.mjs hol-labs <path>`, run from `ajch_platform` — not edited by hand in this repo.

## Validation

CI runs `scripts/validate-content.mjs` against every changed file under `content/hol-labs/` on PR/push.

Local validation:

```bash
node scripts/validate-content.mjs \
  content/hol-labs/index.json \
  content/hol-labs/labs/*.json
```

This checks JSON validity plus the lab schema (required fields, kebab-case `id`, every step has a non-empty `whyItMatters`, `costEstimate.tier` is one of `free|low-cost|paid`) via `validateHolLab()`.

## File ownership and scope

- HOL Lab content lives in this repo only.
- `ajch_platform` tracks the published version through a manifest pin (`content-manifest.json`) — it never edits lab content directly.
- Don't bypass validation before merge.

## Related references

- `ajch_platform`'s `.claude/vertical-registry.json` — this vertical's registry entry (repo, checkout path, agent roles, pipeline shape)
- `ajch_platform`'s `content-manifest.json` — the promotion pin
- `ajch_skillup`, `ajch_aaryaai_blogs`, `ajch_ai_usecases` — sibling vertical repos following the same pattern
- `.github/workflows/validate-content.yml`
- `scripts/validate-content.mjs`
