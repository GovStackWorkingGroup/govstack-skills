---
name: govstack
description: Use when authoring, reviewing, normalizing, or releasing GovStack Building Block specifications, or working with the GovStack Working Groups framework, CFR architecture, and spec/requirement conventions. Trigger on requests like "write a functional requirement for bb-X", "normalize this BB spec", "what classifiers does this requirement need", "set up a new building block", "fix the SUMMARY.md", "cut a new spec version", or any work inside bb-* spec repos, the bb-specification-template, or cfr-architecture.
---

# GovStack specifications

GovStack defines reusable, interoperable digital-government components called **Building
Blocks (BBs)**. Each BB is a standards-track *specification* (not code) developed by a
**Working Group (WG)** through public consensus, published to GitBook at
`https://specs.govstack.global/<short_name>/`. This skill encodes the conventions that
make a spec valid, compliant, and publishable.

When a task touches these conventions, read the authoritative source rather than guessing:
- Process / lifecycle / WG governance → `workinggroups-framework/index.md` (the Meta-Spec).
- Architecture, requirements model, CFRs → `cfr-architecture/` (chapters 1–6).
- Canonical spec layout + per-BB cleanup → `bb-specification-template/STRUCTURE-ANALYSIS.md`
  and `NORMALIZATION-CHECKLIST.md`.
- A clean starting point → `bb-specification-template/bb-govspecs-2-template/`.

## The 14 Building Blocks

cloud-infrastructure, consent, digital-registries, emarketplace, esignature, gis,
identity, information-mediator, messaging, payments, registration, scheduler, wallet,
workflow. The registry of names, repos, `publication_id`s, and versions lives in
`bb-specification-template/building-blocks.yml` — treat it as the source of truth for
identifiers and SSH remotes.

## Canonical spec structure (the contract)

Every `bb-*` repo must match this. Pick ONE form per chapter repo-wide — never keep both a
`N-chapter.md` and a `N-chapter/` folder for the same number.

```
spec/
  README.md
  SUMMARY.md                       # GitBook ToC — MUST match the chapter files exactly
  1-version-history(.md OR /)
  2-description.md
  3-terminology.md
  4-key-digital-functionalities.md
  5-cross-cutting-requirements.md
  6-functional-requirements.md
  7-data-structures.md
  8-service-apis.md
  9-workflows.md
  10-other-resources.md
  .gitbook/assets/                 # ALL binary assets (png/pdf/svg) live here
api/                               # OpenAPI/Swagger definitions only (.yaml + .json)
examples/                         # real impls or a `someApp/` placeholder
test/                             # `test/openAPI/` harness or a `plan.md` placeholder
LICENSE                            # filename `LICENSE`, not LICENSE.txt
README.md  .gitbook.yaml  .prettierrc
```

When normalizing a non-conforming repo: merge unique content into the canonical file
FIRST, then delete the obsolete file, then rebuild `SUMMARY.md` to reference only
surviving files. Common outliers to watch for: off-by-one chapter numbering, `8-apis-and-services.md`
vs `8-service-apis.md`, `9-internal-workflows` vs `9-workflows`, and extra chapters
(`11-key-decision-log`, `12-future-consideration`) that should fold into `10-other-resources.md`.

## Writing requirements (the most important part of a spec)

Requirements drive GovMarket compliance checks and interoperability guarantees, so they
must be precise. Authoritative detail: `cfr-architecture/5-specification-framework/5.3-requirements-model.md`.

Two kinds:
- **Functional Requirements (FR)** — the "WHAT": business-value functionality, including
  Service APIs. May link to one or more **Key Functionalities (KF)** via a `KF: <exact KF name>`
  label in the body. No KF link = global (applies to all KFs).
- **Cross-Functional Requirements (CFR)** — the "HOW": expected of all compliant BBs;
  inherited/extended from GovStack's core CFRs. CFRs normally carry NO KF link.

Rules:
- **Unique, never-reused identifier** per spec (e.g. `govstack-bb-wallet#req-1`). A retired
  requirement becomes DEPRECATED; its number is never recycled.
- **Any change to a requirement's classifiers or intent requires a spec version bump.**

Every requirement carries up to three classifier dimensions. Pick exactly one per
dimension; if omitted, the default applies:

| Dimension | Keywords | Default |
|---|---|---|
| **Requirement level** | REQUIRED · RECOMMENDED · DRAFT · DEPRECATED | RECOMMENDED |
| **Observability** | OBSERVABLE · AUDITABLE | AUDITABLE |
| **Mutability** | IMMUTABLE · EXTENSIBLE · REPLACEABLE · INAPPLICABLE | REPLACEABLE |

- REQUIRED = mandatory for compliance; a single failure disqualifies. RECOMMENDED =
  scored signal on GovMarket. DRAFT = reserved id, no test fails if absent. DEPRECATED =
  frozen for audit.
- OBSERVABLE = verifiable from outside the running system (automatically testable,
  manually testable, or operationally observable). AUDITABLE = needs internal inspection;
  pair it with the evidence artefacts (ADRs, diagrams, audit statements) the implementer
  must produce.
- IMMUTABLE = frozen intent. EXTENSIBLE = base stays fixed, overlays may tighten (never
  broaden). REPLACEABLE = swap implementation, identical contract. INAPPLICABLE = only in
  an extended spec, switches off an inherited rule with recorded rationale.

The 2.0 keywords above replace the old RFC2119 wording (MUST/SHOULD/MAY). RFC2119 prose
may still appear inside requirement text, but the classifier tags are what's normative.

### Requirement form

In the template, FRs are structured YAML front-matter (see
`bb-specification-template/bb-govspecs-2-template/spec/functional-requirements/*.md`):

```yaml
---
fr_id: x
slug: management-of-hardware
name: Management of physical hardware
requirements:
  - identifier: x
    title: Asset management and automation for physical infrastructure
    content: Asset management for the physical infrastructure and automation to deploy...
    requirement_level: REQUIRED
    observability_level:
    mutability_level:
---
```

Inline prose form (also valid):

```
#7 Must return current weather for the API request /api/v1/weather
RECOMMENDED REPLACEABLE
KF: Payment Orchestration
```

## Publication metadata

A spec's `index.yml` carries the release identity (see the template). Key fields:
`publication_urn` (e.g. `govstack:bb:cloud-infrastructure:1.0.1`), `version`,
`version_short_description` (link the PR), `previous_version`, `status`
(draft → … → release), `working_group` (e.g. `govstack:wg:cloud-infrastructure`),
`publish_date`, and `authors`/`editors` with `govstack:person:<id>` identifiers.

## Versioning & releases

- Bump the version whenever a requirement's text intent, any classifier, or a Key
  Functionality name changes. Renaming a KF means updating every `KF:` link that
  references it.
- Cosmetic-only changes (image fixes, indentation, typo) get a patch bump with a
  `version_short_description` that says "Cosmetic:" and links the PR.
- Set `previous_version` and mark the new one `latest: true` in `building-blocks.yml`.

## Standard workflow for common tasks

1. **New requirement** — choose FR vs CFR, assign the next unused id, write a testable
   `content`, set the three classifiers explicitly (don't rely on defaults for normative
   rules), add `KF:` links if scoped, then bump the spec version.
2. **Normalize a BB** — diff against the canonical structure, merge-then-delete outliers,
   move assets into `.gitbook/assets/`, rename chapters to canon, rebuild `SUMMARY.md`.
3. **New BB** — clone `bb-govspecs-2-template`, fill `index.yml` identity, register it in
   `building-blocks.yml`, write the 10 chapters.
4. **Release** — update `index.yml` (status, version, dates, short description), update
   `building-blocks.yml` versions/`latest`, confirm `SUMMARY.md` is consistent.

Always check `SUMMARY.md` matches the actual chapter files after any structural edit —
a mismatch breaks the GitBook build.
