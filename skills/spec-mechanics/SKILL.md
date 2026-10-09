---
name: spec-mechanics
description: Use for the repo mechanics of a GovStack Building Block specification — canonical chapter layout, SUMMARY.md, normalizing a non-conforming bb-* repo, index.yml publication metadata, version bumps and releases. Trigger on "normalize this BB spec", "fix the SUMMARY.md", "set up a new building block", "cut a new spec version", "what goes in index.yml". For writing requirement text or checking classifiers, use the requirements skills instead; for what GovStack concepts mean, use govstack-concepts.
---

# GovStack spec mechanics

The structural and release conventions that make a Building Block specification valid,
compliant, and publishable. This skill is about **file layout and release identity** — not
about what a requirement should say.

Before reading corpus files, check `${CLAUDE_PLUGIN_ROOT}/shared/corpus-map.md` for where
they actually live. Several repos were re-cut in September 2026 and the obvious paths point
at stale copies.

## Which reference to open

| Task | Read |
|---|---|
| Lay out a new spec, or check an existing one | `references/canonical-structure.md` |
| Clean up a repo that doesn't match | `references/normalization.md` |
| `index.yml`, version bumps, cutting a release | `references/release-process.md` |
| Identifiers, URNs, semver rules | `${CLAUDE_PLUGIN_ROOT}/shared/naming-and-versioning.md` |
| Requirement classifiers and form | `${CLAUDE_PLUGIN_ROOT}/shared/requirements-model.md` |

## Standard workflows

**Normalize a BB** — diff against the canonical structure, merge-then-delete outliers, move
assets into `.gitbook/assets/`, rename chapters to canon, rebuild `SUMMARY.md`. Order
matters; see `references/normalization.md`.

**New BB** — clone `bb-govspecs-2-template`, fill `index.yml` identity, register it in
`building-blocks.yml`, write the 10 chapters.

**Release** — update `index.yml` (status, version, dates, short description), update
`building-blocks.yml` versions and `latest`, confirm `SUMMARY.md` is consistent.

## The rule that breaks builds

Always check `SUMMARY.md` matches the actual chapter files after any structural edit. A
mismatch breaks the GitBook build, and it is the single most common failure when
normalizing a repo.

## The rule that breaks compliance

Any change to a requirement's classifiers or intent requires a spec version bump. Renaming
a Key Functionality means updating every `KF:` link that references it.
