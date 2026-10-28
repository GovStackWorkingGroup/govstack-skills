# The GovStack requirements model

Shared by every skill that writes, reviews, or validates a requirement or a functionality file. Authoritative sources, all in `specification-framework/` (see `corpus-map.md`):

| Topic | Chapter |
|---|---|
| What a requirement is, statement wording, identifiers, classifiers, interaction types | `chapters/requirements.md` |
| Capabilities, functionalities, core and extensions | `chapters/functionalities_and_extensions.md` |
| File layout, the functionality file template, `metadata.yml` | `chapters/structure_of_specifications.md` |
| Which release a classifier or interface change needs | `chapters/versioning_and_change_documentation.md` |
| How the overview presents capabilities, functionalities and extensions | `chapters/introductory_chapters.md` |

Requirements drive GovMarket compliance checks and interoperability guarantees, so they must be precise.

> **Proposed, not decided (as of 2026-10-28).** The status/level split is ARCH-ADR-5, core and extensions are ARCH-ADR-7, declared capabilities and `realises` are ARCH-TDR-1, and alternative/conditional requirements (`applies_when`, variation points) are ARCH-ADR-6. None has been decided by the Architecture Committee. The `interaction` classifier has not yet been discussed by the Architecture Working Group. Write to this model, but say so when a claim depends on one of these records, and read the chapter before asserting current behaviour.

## Capabilities, functionalities, requirements

Three layers, each with one job:

| | Capability | Functionality | Requirement |
|---|---|---|---|
| Is | An outcome the BB makes possible for its beneficiaries ("Credential Custody") | A service the BB provides to an actor outside it ("Credential Intake") | One verifiable statement of what the system under test does or is |
| Written for | A decision-maker reading the *Purpose* | An implementer or assessor | An implementer or assessor |
| Declared in | `metadata.yml`, `capabilities` | Its own file in `requirements/` | The `requirements` list in its functionality's front matter |
| Normative | No. Informative, never claimed or assessed | Yes, through its requirements | Yes |
| Core or extension | Neither; follows from the functionalities that realise it | Exactly one of the two | Inherits from its functionality |

- **A capability** states what, never how: no product, vendor, protocol or technology, and no requirements.
- **A functionality** follows the ISO/IEC/IEEE 29148 "feature" organisation. Its file is the unit the specification is organised by; the functionalities named in the overview are the files in `requirements/`, with the same titles. It lists the capabilities it realises in `realises`. The link is many-to-many: a capability may be realised by several functionalities, across the core and extensions, and one functionality may realise several capabilities.
- **A requirement** belongs to exactly one functionality: the file it sits in. There is no `KF:` link any more. The older model scoped requirements to "Key Functionalities" with a `KF: <name>` label; treat that as legacy form when reading pre-2.0 specs.

Rules a reviewer checks:

1. Every capability is realised by at least one functionality, and every functionality realises at least one capability.
2. A capability realised by at least one core functionality is provided by every conformant solution. One realised only by extension functionalities is provided only by solutions that claim one of them, and the *Purpose* says so.
3. A functionality that no actor reaches, directly or through another functionality, is probably an internal mechanism and belongs as requirements of another functionality.
4. Capability ids are unique and never reused. Changing capabilities alone is a **patch** change.

## Core and extensions

Each functionality is `conformance: CORE` (default) or `EXTENSION`, and an extension functionality names its extension in `extension`. Extensions are declared once in `metadata.yml`, next to the capabilities:

```yaml
capabilities:
  - id: custody
    title: Credential Custody
    description: A person keeps the credentials issued to them and can use them whenever they need to.
extensions:
  - id: issuance
    title: Issuance
    description: Obtaining credentials from an Issuer through the issuance exchange.
    role: Credential Holder, towards the Credential Issuer   # optional
    reference_architecture: govstack:ra:credentials          # optional
    requires: []                                             # ids of other extensions
```

- **Core**: what makes a solution an instance of the BB. Never empty, never depends on an extension, and useful on its own.
- **Extension**: a named set of functionalities a solution may claim in addition to the core. All or nothing: a solution that claims it meets every `REQUIRED` requirement in it and in the extensions it `requires`.
- An extension may add to a core functionality's behaviour when claimed; it may not weaken or replace a core requirement. Requirements of one functionality are never split between core and extension.
- Requirements of an unclaimed extension are **not applicable**: neither passed nor failed, and left out of the RECOMMENDED share GovMarket reports. GovMarket shows claimed extensions as facts ("Wallet 2.0: core, Issuance, Presentation"), not a score.

Make a functionality an extension when some conformant products legitimately lack it, when it is an obligation towards a role in a domain (a Domain Reference Architecture carries no requirements, so those obligations live in the BB spec, usually as an extension), or when it depends on infrastructure not every deployment has. Warning signs: every product would claim it (core in disguise); it has no `REQUIRED` requirement (write `RECOMMENDED` core requirements instead); the core cannot stand alone; it can be assessed without the core (a separate BB); extensions multiply per combination (a variation point instead).

Changes to extensions: adding one is MINOR; moving a functionality between core and an extension, deprecating an extension, or adding to `requires` is MAJOR.

Don't confuse the terms that sound alike:

| Term | Means | Unit |
|---|---|---|
| `extends` | One specification inherits another, and may tighten it. Every BB spec extends the CFRs | Whole specification |
| Profile | A separate specification that extends a BB spec and narrows it for one context | Whole specification |
| Extension | A named set of functionalities inside one BB spec, claimable on top of the core | Functionalities |
| Variation point | A requirement met by meeting some of its alternatives (ARCH-ADR-6) | Requirement |

Never call an extension "optional": in GovStack usage "optional" means `RECOMMENDED`.

## Two kinds of requirement

- **Functional requirements**: in the BB spec's functionality files. Their subject is always the system under test, as delimited in the overview's *System under test*.
- **Cross-functional requirements (CFR)**: shared by every BB (transport security, logging, …), in `govstack:spec:cfr`. A BB spec inherits them and must **not** restate them, except to tighten an `EXTENSIBLE` one, citing it.

## Writing the statement

A requirement states what the system does, not how, optionally qualified by a **condition** and bounded by a **constraint**. Sentence pattern (ISO/IEC/IEEE 29148, 5.2.4):

> [Condition] [Subject] [Action] [Object] [Constraint of action]

*When a Verifier requests a presentation, the Wallet shows the Holder every requested attribute before any attribute is sent.*

- Only subject and action are mandatory. Conditions and constraints that don't fit one sentence go in `documentation`, labelled "Condition:" / "Constraint:".
- Obligation is carried by `level`, never by the verb. Plain present tense ("The Wallet keeps …") is preferred; RFC 2119 keywords are allowed but not normative. Use capitalised RFC 2119 keywords only for obligations inside `documentation`.
- Avoid: superlatives, subjective terms, vague pronouns, and/or, open-ended terms ("etc.", "support for"), loopholes ("if possible"), unqualified totality unless meant and testable, incomplete references ("according to the standard").
- Common mistakes: a person as subject ("A holder MUST be able to …"); two obligations in one ("stores and encrypts"); a goal ("is secure"); restating an inherited CFR; naming a product.

A requirement is ready for review when it is necessary, appropriate, unambiguous, complete, singular (one subject, action, object, interaction type), feasible, verifiable (`observability` set), correct (doesn't contradict an inherited `IMMUTABLE` rule), conforming, and traceable. The full checklist is in `chapters/requirements.md`, *Checklist*. As a set, the requirements leave no functionality empty, contain no contradictions, and use one term per concept.

## Identifiers

Cited as `<spec id>#req-<n>`, e.g. `govstack:spec:bb:wallet#req-7`. Procurement documents and laws cite these, so they must resolve to the same rule years later.

1. A number is issued at **acceptance** (`CANDIDATE` → `ACCEPTED`), not when drafting. Drafts have no `id`.
2. A new requirement takes the next free number: one above the highest ever used, including deprecated and withdrawn ones. Check the default branch, not just the last release.
3. Never removed (a retired published rule becomes `DEPRECATED`), never reused, never renumbered (moving files or reordering keeps the number).
4. The intent of an `IMMUTABLE` requirement never changes; its wording may be clarified.
5. One number sequence per specification, until the Architecture WG decides otherwise.

The compiler enforces 2–3 and flags every wording change to an `IMMUTABLE` requirement for review.

## Classifiers

Each takes exactly one value; a missing value takes the default, and writing the default explicitly is not a change.

| Field | Values | Default | Answers |
|---|---|---|---|
| `id` | a number | none until accepted | Which requirement |
| `status` | DRAFT · CANDIDATE · ACCEPTED · WITHDRAWN · PUBLISHED · DEPRECATED | DRAFT | Where it stands in its life cycle |
| `level` | REQUIRED · RECOMMENDED | RECOMMENDED | Whether a solution fails without it |
| `observability` | OBSERVABLE · AUDITABLE | AUDITABLE | Verified from outside the running system, or by inspecting evidence |
| `mutability` | IMMUTABLE · EXTENSIBLE · REPLACEABLE · INAPPLICABLE | REPLACEABLE | What an extending spec may do with it |
| `interaction` | SERVICE_API · CONSUMED_API · PROTOCOL · EVENT · USER_INTERFACE · DEVICE · FILE · INTERNAL | none | Across which boundary it is exercised, or none |

`DRAFT` and `DEPRECATED` are **statuses, not levels** (ARCH-ADR-5). The old four-value level enumeration (REQUIRED · RECOMMENDED · DRAFT · DEPRECATED) is superseded; a draft already records its intended level.

**Level.** REQUIRED: a solution failing a single published REQUIRED requirement, in the core or in a claimed extension, is not compliant. RECOMMENDED: GovMarket reports the share met.

**Status.** Only `PUBLISHED` requirements take part in compliance. Released specs may carry unnumbered requirements, marked not normative and without a compliance test, so implementations can experiment.

| Status | Numbered | Meaning |
|---|---|---|
| DRAFT | No | Being written; anything may change |
| CANDIDATE | No | Submitted to the WG for review |
| ACCEPTED | Yes | Approved and merged, not yet released |
| WITHDRAWN | If it was accepted | Abandoned before publication; its number is skipped for good |
| PUBLISHED | Yes | In a released version |
| DEPRECATED | Yes | Published once, no longer applies; kept for audit |

Allowed transitions: DRAFT→CANDIDATE, DRAFT→WITHDRAWN, CANDIDATE→DRAFT, CANDIDATE→WITHDRAWN, CANDIDATE→ACCEPTED (number issued), WITHDRAWN→DRAFT (unnumbered only), ACCEPTED→PUBLISHED, ACCEPTED→WITHDRAWN, PUBLISHED→DEPRECATED, DEPRECATED→PUBLISHED. Nothing else: a numbered requirement never returns to DRAFT or CANDIDATE, and a published one is deprecated, never withdrawn.

**Observability.** OBSERVABLE: verifiable through API calls, protocol exchanges or behavioural tests; automatically testable, manually testable, or operationally observable, in that order of preference. AUDITABLE: needs architecture docs, source, config, policies or audit reports; list in `documentation` the evidence a vendor produces. Prefer an observable formulation.

**Mutability.** IMMUTABLE: frozen intent. EXTENSIBLE: an extending spec may tighten, never broaden. REPLACEABLE: the way it is met may be swapped, contract unchanged. INAPPLICABLE: only in an extending spec, switches off an inherited EXTENSIBLE or REPLACEABLE rule with a recorded reason. An extending requirement cites its base: `govstack:spec:bb:wallet#req-1 extends govstack:spec:cfr:data#req-3`.

## Interaction type

Where a tester has to stand to see the requirement met. Decide in order, stopping at the first yes:

1. An obligation of a role in an exchange defined by a Domain Reference Architecture or an external standard → `PROTOCOL` (even if carried over API calls)
2. Another party calls the system under test → `SERVICE_API`
3. The system under test calls another party → `CONSUMED_API`
4. The exchange is through published messages → `EVENT`
5. The other party is a person → `USER_INTERFACE`
6. The other party is hardware → `DEVICE`
7. It concerns a file or data package read or produced → `FILE`
8. Otherwise → `INTERNAL`

Rules:

- Exactly one type per requirement. Needing two usually means two requirements (e.g. a file uploaded through an API is one `SERVICE_API` requirement for the transfer and one `FILE` for the format).
- `SERVICE_API` names its operation in `api_operation_id`, which exists in `resources/api.yml`.
- `CONSUMED_API`, `PROTOCOL`, `DEVICE` or `FILE` with a contract defined elsewhere names it in `contract` (publication id plus operation/role/section, or a document in `references.md`).
- Every interface in the overview's *Interfaces* is realised by at least one requirement of the matching type, and every non-`INTERNAL` requirement realises a listed interface.
- Interfaces inherited from the CFRs (logging, monitoring) get no interaction type in the BB spec.
- `INTERNAL` is not `AUDITABLE`: choose `observability` separately. `USER_INTERFACE` says what a person is shown or asked, not how the screen looks. When a protocol is realised by an extension, all its `PROTOCOL` requirements sit in that extension.

## Form

A functionality file in `requirements/` carries its requirements in YAML front matter, and describes the functionality, its workflows and data structures in the body (free text, no embedded requirements):

```yaml
---
title: Credential Issuance            # as in the overview's Functionalities table
description: |                        # business view, no requirements language (was `capability`)
   The holder obtains credentials from an issuer through the issuance exchange.
realises: [custody]                   # capability ids from metadata.yml
conformance: EXTENSION                # CORE (default) | EXTENSION
extension: issuance                   # only when conformance is EXTENSION
requirements:
  - id: '4'                           # absent until ACCEPTED
    status: ACCEPTED
    level: REQUIRED
    observability: OBSERVABLE
    mutability: IMMUTABLE
    interaction: CONSUMED_API
    contract: govstack:ra:credentials#status-lookup
    statement: The Wallet checks the revocation status of a credential with the Trust Registry before presenting it.
    documentation: |
      Condition: the credential carries a status reference.
      Constraint: a status that cannot be retrieved is treated as unknown, and the credential is not presented as valid.
    api_operation_id:
    test:
---
```

Legacy forms you may meet in pre-2.0 or unmigrated specs, to be migrated rather than copied: `capability:` as the functionality description (read as `description`, compiler warns); `fr_id`/`identifier`/`content`/`requirement_level`/`observability_level`/`mutability_level` field names; a `type` field (`FUNCTIONAL`, `WEBSERVICE`, `INTERFACE`), which `interaction` is proposed to replace; inline `#7 … RECOMMENDED REPLACEABLE` prose with a `KF:` label.

## Writing a new requirement

1. Decide it is a functional requirement of this BB and not a CFR it already inherits.
2. Put it in the functionality it realises. If none fits, a functionality may be missing; a new one must realise a capability and be reached by an actor.
3. Write one sentence in the pattern above, with the system under test as subject. Move conditions and constraints to `documentation`.
4. Set `level`, `observability`, `mutability` and `interaction`; add `api_operation_id` or `contract` as the type requires; for AUDITABLE, list the evidence.
5. Leave `status: DRAFT` and no `id`. The number is issued on acceptance.
6. Classify the release it will need via `chapters/versioning_and_change_documentation.md`, *Classifier changes*. Publishing a REQUIRED requirement is MAJOR, a RECOMMENDED one MINOR; status moves among DRAFT/CANDIDATE/ACCEPTED/WITHDRAWN are PATCH; changing `level` either way is MAJOR.

## Scope boundaries

- A specification is for a **software component, never a government service**. Requirements that describe how a ministry should run a service are out of scope.
- Specs are technical and **must not block legal compliance** (eIDAS, GDPR), but never confer it. Legal material belongs in optional *Guides*, not specifications.
- If a choice has **no impact on business function, it must not be a requirement**. A programmer's preference is never a requirement.
