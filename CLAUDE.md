# ajch_hol_labs

This repo is a promoted content vertical — the canonical source for Aarya's HOL Labs library. It is designed to be usable standalone, without `ajch_platform` checked out alongside it.

## Content changes go through the pipeline — never edit `content/` directly

Any addition or edit to `content/hol-labs/` (lab files, `index.json`) **must** go through the agent pipeline defined in `.claude/agents/`:

**HOL Lab Lead → HOL Lab Writer → AppSec Engineer (Security Gate, pre- and post-build) → HOL Lab Publisher**

- If asked to add, edit, or fix a lab, invoke **HOL Lab Lead** — do not write to `content/hol-labs/` directly, even for a "quick" or "small" change.
- HOL Lab Lead never writes files itself; it orchestrates HOL Lab Writer (drafts, no file I/O), AppSec Engineer (hard gate, must `PASS ✓` before and after any write), and HOL Lab Publisher (the only role with `Write`/`Edit` access, scoped to `content/hol-labs/` only).
- The shared pipeline contract is documented once in `.claude/skills/vertical-pipeline/SKILL.md` — read it before authoring or modifying any of the four agent files.

This is backed by `.github/workflows/validate-content.yml`, which runs `scripts/validate-content.mjs` (including the lab schema check) on every PR — but that's a post-hoc catch, not a substitute for routing through the pipeline in the first place.

## Every lab must earn its place

A lab is not generic content: `problemStatement` must be a real, grounded scenario (never invented), `approachRationale` must name a real rejected alternative, and every step's `whyItMatters` must explain a genuine trade-off. A lab with no real problem behind it should not be authored — HOL Lab Writer is instructed to flag that back to HOL Lab Lead rather than fill in filler. Cross-link research (exams, blog posts, use cases, other labs) is a mandatory research step, not an optional field — see `hol-lab-writer.md`.

## Agent files are synced from `ajch_platform`, not hand-edited here

`.claude/agents/{appsec-engineer,hol-lab-lead,hol-lab-writer,hol-lab-publisher}.md` and `.claude/skills/vertical-pipeline/SKILL.md` are canonical in `ajch_platform` and mechanically synced into this repo via `node scripts/sync-vertical-agents.mjs hol-labs <path-to-this-repo>` (run from `ajch_platform`). Don't hand-edit them here — changes will be overwritten on the next sync and this repo will drift from the canonical pipeline definition again.

## Scope

- HOL Lab content lives in this repo only — `ajch_platform` tracks it through a manifest pin (`content-manifest.json`) and never edits it directly.
- See `README.md` for the full publishing model and validation details.
