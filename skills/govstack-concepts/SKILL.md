---
name: govstack-concepts
description: Orientation to what GovStack is, what problem it solves, and the domain model behind it — Building Blocks, specifications vs. software, interoperability maturity states, cross-functional requirements, Working Groups, GovMarket. Read this BEFORE reasoning about GovStack content (writing prose, arguing architecture, answering "what is X", reviewing a chapter for correctness). For the mechanics of authoring a spec — file layout, classifiers, version bumps — use the spec-mechanics skill instead.
---

# What GovStack is

GovStack is an international initiative that publishes **open, vendor-neutral technical
specifications** for the reusable software components of digital public infrastructure.
It does not build, sell, host, or certify software. Its output is documents.

The one-sentence definition used across the corpus (`https://specs.govstack.global/architecture/2-common-terminology.md`):

> GovStack is a collaborative initiative that provides a reference architecture for digital
> government systems. It promotes a "whole-of-government" approach and offers a methodology
> for leveraging reusable technology components ("Building Blocks") so that governments can
> create interoperable digital platforms to address high-priority use cases.

## The problem it exists to solve

Governments build system by system, ministry by ministry. Each ministry serves its own
mandate, so over time a country accumulates dozens of isolated systems with overlapping
data and duplicated functions that cannot coordinate without manual work. Citizens
experience this as being asked for the same information repeatedly; institutions
experience it as integration projects that consume budget without producing anything
reusable.

The scaling argument is the core of the pitch: **50 systems with no common standards face
up to 1,225 unique integration paths; the same 50 systems against a shared framework face
50 connections to one set of rules.** Point-to-point integration works for a handful of
connections and collapses at national scale.

Four benefits are claimed, and they are the frame for most GovStack prose:
citizen value (life events trigger coordinated responses without the citizen acting as
courier between agencies), institutional efficiency (reuse instead of rebuilding),
vendor independence (open interfaces mean systems can be replaced without rewriting every
connection), and trust/accountability (governed channels leave audit trails).

The recurring analogy is the railway gauge: a state that defines the track width does not
need to own the train factory. **It owns the standard.** That is the architectural
definition of digital sovereignty used here — sovereignty as a property of construction,
not a political aspiration. See `https://govstack.global/news/digital-sovereignty-in-a-fragmented-world-how-specifications-empower-governments-to-be-in-control/` for the long form.

## The central distinction: specification, not software

This is the mistake most worth avoiding. Get these three apart and most confusion
dissolves:

| Term | What it is |
|---|---|
| **Building Block Specification** | A document. Normative requirements + API contracts for one class of component. The GovStack deliverable. |
| **Building Block Software** | An implementation of that spec by a vendor, a government, or an open-source project. Not GovStack's output. |
| **Building Block** | Loosely used for either. In careful writing, prefer one of the two above. |

A further distinction, from `https://specs.govstack.global/architecture/5-specification-framework/5.1-specification-scope`: **a specification is for a
software component, never for a government service.** A service is the value-delivering
offering (eligibility, obligations, SLAs, end-to-end process, possibly non-digital
channels). A Building Block is the software part that automates routines inside it.
Requirements that describe how a ministry should run a service are out of scope; the spec
covers only the technical component.

Related scope rule: GovStack specs are **technical and must not block legal compliance**
(eIDAS, GDPR), but they never confer it — legal compliance involves process and
organizational practice beyond software. Legal material lives in optional *Guides*, not
in specifications.

## What a Building Block is

Criteria (derived from the ITU/DIAL SDG Digital Investment Framework and the DPGA
definition) — a Building Block:

- provides a basic digital service at scale, reusable across multiple use cases and contexts;
- is a component of a larger **Digital Service System**;
- can be combined with others to form a country's DPI;
- **may be open source or proprietary** — Building Blocks are not automatically DPGs.

Four characteristics: **autonomous** (standalone, though internally composed of many
services), **generic** (flexible across sectors), **interoperable** (combines with others),
**iteratively evolvable** (improvable while in production). Each spec targets roughly the
*minimum* functionality needed to do its job, so it stays useful alone and extensible in
combination.

Two categories — a governance distinction only, not a content one:

- **Foundational**: digital identity, information-mediator, workflow, and registry.
  Almost everything else depends on them (registry for reuse potential rather than
  hard dependency).
- **Feature**: payments, messaging, GIS, e-signature and the rest — standalone functions
  that improve the stack without being prerequisites.

The 14 current BBs: cloud-infrastructure, consent, digital-registries, emarketplace,
esignature, gis, identity, information-mediator, messaging, payments, registration,
scheduler, wallet, workflow.

## Interoperability: three cumulative maturity states

Interoperability is not binary, and knowing which state a claim refers to is usually the
difference between a correct and an incorrect statement (`specs.govstack.global/architecture/4-interoperability-architecture/4.1-govstack-interoperability`):

1. **Protocol interoperability** — same transport, formats, encoding, versioning,
   auth patterns (HTTPS/TLS 1.3, JSON over REST, UTF-8, ISO 8601 UTC). Systems exchange
   data without custom adaptors, but endpoint *meaning* is still software-specific, so
   every integration still needs a bilateral agreement. Addressed by **govstack-cfr**.
2. **Cross-functional interoperability** — same service API contracts per functional
   domain: same endpoints, parameters, response schemas, error codes. The practical test
   is **replaceability**: swap one compliant implementation for another without changing
   the consumer's code. The data behind the API still differs (a different population
   registry holds different citizens); the contract does not. Addressed by
   **govstack-bb-\*** specs. This is the level at which competitive procurement,
   compliance testing and GovMarket operate.
3. **Whole-of-government interoperability** — the target state. Adds binding
   organizational, legal and semantic standards, so a response is not merely structurally
   correct but semantically consistent. Requires data-sharing agreements, common data
   models, a national service catalogue, governance bodies with authority. Addressed by
   **PAERA**, not by the technical specs.

The states are cumulative, but work on the legal/organizational layers should start in
parallel — they take longer to establish than the technical ones.

Three architecture scopes are distinguished alongside this: **Government Architecture**
(whole of government, governance/legal/digital layers — PAERA's territory), **System
Architecture** (interaction between multiple BBs and applications — C4 context/container),
and **Component Architecture** (the inside of one BB — C4 component). The CFR/Architecture
specification covers the latter two.

GovStack is explicitly **not an interoperability framework** — it complements one, and is
recommended for adoption *inside* a government's own framework.

## Cross-functional requirements and the inheritance model

**CFR** = what used to be called non-functional requirements; the term follows Fowler and
Newman's shift away from "non-functional", with a consequence that matters: *if a choice
has no impact on business function, it must not be a requirement.* A programmer's
preference is never a requirement.

The model is deliberately object-oriented: **every Building Block specification extends
`govstack-cfr`**, the way a class extends a base class. A BB spec inherits the whole
baseline — security, deployment, observability, data handling — and adds its own
functional requirements on top. It may tighten or elaborate inherited rules; it may not
contradict or weaken them. A BB with 5 of its own requirements still carries all of the
core ones. BB specs must **not** copy-paste core requirements.

Written form: `govstack-bb-wallet-2.0.0 extends govstack-cfr-2.0.0`. If omitted, the
extension is assumed — no BB can opt out.

CFR domains: development, deployment, architecture, quality, security, data
(`govstack-cfr-development`, `-deployment`, `-architecture`, `-quality`, `-security`, `-data`).

## The nine architecture principles

Openness & neutrality · modular and flexible architecture · interoperability ·
sustainability & robustness · security & privacy · data ownership & portability ·
user-centered design · observability & maintainability · policy as code.

These are framed as the **architectural foundation for digital sovereignty**: a system
built consistently on them is portable across providers, inspectable, and free of hidden
single-vendor dependency. Sovereignty concerns outside architecture — hosting location,
legal jurisdiction, operational continuity — are explicitly *not* covered by technical
requirements.

Two principles are load-bearing in a way worth remembering: specifications standardize
**interfaces and functionalities, never implementations**; and open source is *recommended*
for governments but **never required** for compliance.

## Data exchange and the Information Mediator

GovStack advocates semi-decentralized SOA and fully decentralized event-driven
microservice patterns — decentralized architecture where exchange is mediated rather than
centrally hubbed. The canonical reference model is Estonia's X-Road.

Any communication **across the internet** between deployments should go through an
**Information Mediator** (co-located BBs may use direct API calls). An Information Mediator
must provide: address management, message routing, access rights management,
organization-level and machine-level authentication, transport encryption, time-stamping,
digital signature of messages, logging, error reporting, monitoring/alerting, and service
registry/discovery. Its gateway component is the **Security Server**.

Around this sit the architecture components: domain-dependent ones (service application
frontend/backend, local repositories, BB emulators, domain-adapted BBs such as a Registry
turned into a Population Registry) and domain-agnostic ones (generic BB software, Security
Servers, **adaptors** that map an existing product's API onto the GovStack contract, and
non-BB legacy software).

## How specifications get made: Working Groups

Specifications are produced by **Working Groups** — open communities of experts, operating
under **annual charters** that must be renewed, working by **consensus first** with voting
as a fallback. Roles: *Members* (anyone participating), *Representatives* (one or two per
WG; the points of contact, who also sit in the Architecture Working Group), and
*Facilitators* (who run the group's operations).

The **Specifications Track** — for a new spec or a new major version:

1. Specification Proposal → 2. Specification Draft → 3. Release Candidate →
4. Published Specification → 5. Obsoleted Specification

Minor versions skip the Proposal phase and use an internal WG review. A Proposal must pass
a Product Soundness Review, an Architectural Soundness Review, and Governance Committee
approval. Review must be open where possible, with **at least 2 reviewers who are neither
credited in the spec nor from an active member organization** — the independence rule.
Obsoleting requires an ADR approved by the Architecture Committee and the Governance
Committee.

Vendor-neutral does **not** mean vendor-excluding: vendor experts are actively wanted for
their implementation experience; what is prohibited is any single vendor steering a
specification. The stated goal is a plurality of implementations per spec.

Authoritative source: `govstack-process/chapters/govstack_working_groups.md` and
`publication_tracks.md` (see `${CLAUDE_PLUGIN_ROOT}/shared/corpus-map.md`). These supersede
the older `workinggroups-framework/index.md`, which is an incomplete v0.0.3 draft.

## Who the specifications are for

Governments (as a ready-made architectural baseline, adoptable selectively and
referenceable from national interoperability frameworks and procurement) · donors (World
Bank, Gates Foundation, GIZ/BMZ, ITU, DIAL, ESTDEV, European Commission, USAID, UNICEF,
UNDP — as testable conditions in funding agreements) · the private sector, by three paths:
building new software against a spec, retrofitting existing software, or wrapping
unmodifiable software in an **adaptor** (a transitional measure that must itself be
CFR-compliant) · **GovMarket** · specification developers · academia · software architects ·
AI-assisted development.

**GovMarket** is GovStack's solution marketplace and the reason requirement precision has
teeth: to be listed, a solution must meet **100% of REQUIRED requirements** of both
`govstack-cfr` and the relevant BB spec; RECOMMENDED requirements are scored as a
compliance percentage shown to buyers. This is why a requirement's classifiers are
normative metadata and not editorial garnish.


## Lookups

This skill is orientation. Detail lives alongside it:

| Need | Read |
|---|---|
| Terms this corpus uses in a non-obvious way | `references/glossary.md` |
| Citable sources for a claim | `references/further-reading.md` |
| Where the corpus lives on disk, and which copies are stale | `${CLAUDE_PLUGIN_ROOT}/shared/corpus-map.md` |
| Naming schema, URNs, semver, extension rules | *(under revision since 2026-09-20 — `shared/naming-and-versioning.md` is not reliable; ask the user)* |
| Requirement classifiers and form | `${CLAUDE_PLUGIN_ROOT}/shared/requirements-model.md` |

When a claim matters, read the source file rather than relying on this summary — the corpus
is under active revision.

**Currently being rewritten (2026-09-20), so treated as unavailable:** the `spec-mechanics`
skill (disabled via `skillOverrides` in `~/.claude/settings.json`), `shared/naming-and-versioning.md`,
and `references/canonical-structure.md`. Do not apply spec layout, naming or versioning rules
from memory — ask the user.
