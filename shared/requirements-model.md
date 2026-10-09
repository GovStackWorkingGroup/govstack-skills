# The GovStack requirements model

Shared by every skill that writes, reviews, or validates a requirement. Authoritative
source: `specification-framework/chapters/requirements_model.md`, with wording quality in
`crafting_well_formed_requirements.md` (see `corpus-map.md` for how to reach them).

Requirements drive GovMarket compliance checks and interoperability guarantees, so they
must be precise.

## Two kinds

- **Functional Requirements (FR)** — the "WHAT": business-value functionality, including
  Service APIs. May link to one or more **Key Functionalities (KF)** via a
  `KF: <exact KF name>` label in the body. No KF link = global (applies to all KFs).
- **Cross-Functional Requirements (CFR)** — the "HOW": expected of all compliant BBs;
  inherited/extended from GovStack's core CFRs. CFRs normally carry NO KF link.

A BB spec must **not** copy-paste core CFRs — it inherits them. See the inheritance model
in the `govstack-concepts` skill.

## Identifiers

- **Unique, never-reused identifier** per spec (e.g. `govstack-bb-wallet#req-1`). A retired
  requirement becomes DEPRECATED; its number is never recycled.
- **Any change to a requirement's classifiers or intent requires a spec version bump.**
- Individual requirements are not separately versioned — semver applies to the whole document.

## Classifiers

Every requirement carries up to three classifier dimensions. Pick exactly one per
dimension; if omitted, the default applies. Don't rely on defaults for normative rules —
set all three explicitly.

| Dimension | Keywords | Default |
|---|---|---|
| **Requirement level** | REQUIRED · RECOMMENDED · DRAFT · DEPRECATED | RECOMMENDED |
| **Observability** | OBSERVABLE · AUDITABLE | AUDITABLE |
| **Mutability** | IMMUTABLE · EXTENSIBLE · REPLACEABLE · INAPPLICABLE | REPLACEABLE |

**Requirement level.** REQUIRED = mandatory for compliance; a single failure disqualifies.
RECOMMENDED = scored signal on GovMarket. DRAFT = reserved id, no test fails if absent.
DEPRECATED = frozen for audit.

**Observability.** OBSERVABLE = verifiable from outside the running system (automatically
testable, manually testable, or operationally observable). AUDITABLE = needs internal
inspection; pair it with the evidence artefacts (ADRs, diagrams, audit statements) the
implementer must produce.

**Mutability.** IMMUTABLE = frozen intent. EXTENSIBLE = base stays fixed, overlays may
tighten (never broaden). REPLACEABLE = swap implementation, identical contract.
INAPPLICABLE = only in an extended spec, switches off an inherited rule with recorded
rationale.

The 2.0 keywords above replace the old RFC2119 wording (MUST/SHOULD/MAY). RFC2119 prose may
still appear inside requirement text, but the classifier tags are what's normative.

## Form

Structured YAML front-matter, as in
`workinggroups-framework/bb-specification-template/bb-govspecs-2-template/spec/functional-requirements/*.md`:

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

## Writing a new requirement

1. Choose FR vs CFR.
2. Assign the next unused id — never reuse a retired one.
3. Write a testable `content`.
4. Set all three classifiers explicitly.
5. Add `KF:` links if the requirement is scoped rather than global.
6. Bump the spec version.

## Scope boundaries

- A specification is for a **software component, never a government service**. Requirements
  that describe how a ministry should run a service are out of scope.
- Specs are technical and **must not block legal compliance** (eIDAS, GDPR), but never
  confer it. Legal material belongs in optional *Guides*, not specifications.
- If a choice has **no impact on business function, it must not be a requirement**. A
  programmer's preference is never a requirement.
