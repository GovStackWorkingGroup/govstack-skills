# Where the GovStack corpus actually lives

Skills must not use bare relative paths like `architecture/introduction/common-terminology.md` —
those resolve only when the cwd happens to be the corpus root. Use the paths below, or the
published URL when the claim concerns released content rather than work in progress.

**Local corpus root:** `~/programming/govstack/`
(also reachable as `~/Documents/programming/govstack/` — the same tree.)

## Authoritative sources

| Topic | Local path | Remote |
|---|---|---|
| Architecture, principles, interoperability, CFRs | `architecture/` | `GovStackWorkingGroup/cfr-architecture` |
| Requirements model, classifiers, well-formed requirements | `specification-framework/chapters/` | `GovStackWorkingGroup/specification-framework` |
| WG governance, publication tracks, namespaces | `govstack-process/chapters/` | `GovStackWorkingGroup/meta-specification` |
| BB spec template, reference template | `bb-template/` | — |
| transition trackng, normalization checklists | `transition-bb-specs` | — |
| Published specs | — | https://specs.govstack.global/ |

## The GovSpecs 2.0 split

The CFR/Architecture is being split into newer repos. The split-out repos will contain updated information as follows:

- `specification-framework/chapters/requirements.md` supersedes `5.3-requirements-model.md`. It holds what a requirement is, statement wording (the former `crafting_well_formed_requirements.md`), identifiers, the classifiers including `status` and `interaction`, and the front-matter form
- `specification-framework/chapters/functionalities_and_extensions.md`: capabilities, functionalities, core and extensions. No equivalent in the old chapter 5
- `specification-framework/chapters/structure_of_specifications.md`: repository outline, `metadata.yml` blocks, and the functionality file template
- `specification-framework/chapters/introductory_chapters.md`: overview and scope, superseding `5.1-specification-scope.md` (the former `specifications_scope.md`)
- `specification-framework/chapters/versioning_and_change_documentation.md`: release types, including which release each classifier, interface and extension change needs
- `specification-framework/ADR/`: the decision records behind these chapters (ARCH-ADR-5 status/level, ARCH-ADR-7 core and extensions, ARCH-TDR-1 capabilities). All proposed, none decided yet
- `govstack-process/chapters/govstack_working_groups.md`, `publication_tracks.md`, `govstack_namespaces.md`

Inside `specification-framework/`, `research/` is internal working material. Never cite it or point readers to it; the chapters above are the sources.

## Stale copies or non-relevant folders — do not read these

- Everything under `archived/`
- Everything under `govstack-comms/` this is for other purposes
- Folders `bb-payments-rebuilt` and `bb-wallet` these are separate parallel projects.

## Maintaining this file

This map is the single place any skill records a corpus location. If a path moves, fix it
here rather than in each SKILL.md. Verify before trusting: the tree is under active
reorganization and several repos were re-cut in September 2026.
