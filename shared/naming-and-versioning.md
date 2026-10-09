# Naming and versioning

> **⚠ UNDER REVISION since 2026-09-20 — do not rely on this file.**
> It is being rewritten. Do not cite it, do not apply its rules, and do not use it to
> validate or normalize a spec. Ask the user for the current rules instead.
> Remove this banner when the rewrite lands.


Shared by every skill that references a spec, a requirement, or a release. Authoritative
source: `govstack-process/chapters/govstack_namespaces.md` (see `corpus-map.md`).

## Naming schema

`govstack-[document-type]-[name]` — e.g. `govstack-bb-wallet`.

- With version: `govstack-bb-workflow-2.0.0`
- A requirement: `govstack-bb-wallet-fr#req-7`
- A working group: `govstack:wg:cloud-infrastructure`
- A publication URN: `govstack:bb:cloud-infrastructure:1.0.1`
- A person: `govstack:person:<id>`

**Reserved nodes** that cannot appear as a BB name: `govstack`, `bb`, `fr` (functional
requirements), `cfr` (cross-functional requirements).

Dash-separated numbers are reserved for versions — `govstack-bb-wallet-2` is illegal,
`govstack-bb-wallet2` is not.

## The 14 Building Blocks

cloud-infrastructure, consent, digital-registries, emarketplace, esignature, gis, identity,
information-mediator, messaging, payments, registration, scheduler, wallet, workflow.

The registry of names, repos, `urn`s, and versions lives in
`workinggroups-framework/bb-specification-template/building-blocks.yml` — treat it as the
source of truth for identifiers and SSH remotes.

Each published spec lands at `https://specs.govstack.global/<short_name>/`, where
`<short_name>` is the BB name from the list above.

## Versioning

Semantic versioning applies to the **whole document**; individual requirements are not
separately versioned.

- **Major** — may break an integration
- **Minor** — backward-compatible addition
- **Patch** — corrections with no behaviour change

Bump the version whenever a requirement's text intent, any classifier, or a Key
Functionality name changes. Renaming a KF means updating every `KF:` link that references
it. Cosmetic-only changes (image fixes, indentation, typo) get a patch bump with a
`version_short_description` that says "Cosmetic:" and links the PR.

Older specs use `bb-*` names and a `YYQN` version scheme (25Q2 expected to be the last);
these are being migrated to 2.0.0 conventions.

## Extension

Every BB specification extends `govstack-cfr`, written as
`govstack-bb-wallet-2.0.0 extends govstack-cfr-2.0.0`. If omitted, the extension is
assumed — no BB can opt out.

Specs may be further extended (`govstack-bb-wallet-europe extends govstack-bb-wallet`) with
declared reclassifications, mirroring HL7 FHIR profiling: IMMUTABLE rules cannot be altered,
EXTENSIBLE rules can be tightened but never broadened, REPLACEABLE rules can be swapped or
made INAPPLICABLE.
