# Vocabulary an agent gets wrong most often


- **Building Block Specification** ≠ **Building Block Software**. The initiative publishes
  the former.
- **Capability** ≠ **Functionality**. A capability is an outcome for beneficiaries, named in the *Purpose* ("Credential Custody"); it is informative and carries no requirements. A functionality is a service to an actor ("Credential Intake"), has its own file in `requirements/`, holds the requirements, and lists the capabilities it `realises` (many-to-many). The older term **Key Functionality (KF)**, with `KF:` links on requirements, is legacy: a requirement now belongs to the functionality whose file it sits in.
- **Core** vs **extension**. The core is the functionalities every conformant solution meets. An extension is a named set of functionalities a solution may claim on top of the core, and must then meet in full. Never call an extension "optional": "optional" means `RECOMMENDED`.
- **extends** vs **profile** vs **extension** vs **variation point**. `extends` and profile work on whole specifications (inherit; narrow for one context). An extension is a set of functionalities inside one spec. A variation point is one requirement met through some of its alternatives.
- **Status** ≠ **level**. `DRAFT` and `DEPRECATED` are lifecycle statuses, not requirement levels; the level is only `REQUIRED` or `RECOMMENDED`.
- **Digital Service System** — one system delivering a government service, composed of
  applications, BBs and microservices. A **Service** is the broader offering it supports.
- **DPI** vs **DPG** — DPI is the national infrastructure layer; a DPG is a specific
  open-licensed public good. BBs may be proprietary, so a BB is not necessarily a DPG.
- **Cross-functional** (current) vs **cross-cutting** (older GovStack wording) vs
  **non-functional** (legacy industry term) — same thing; use the first.
- **Member** has a specific technical meaning too: an organization authorized to
  communicate via the Information Mediator in a given implementation.
- **Adaptor** — external mapping component, not a modification of the source software.
- **PAERA** — Public Administration Ecosystem Reference Architecture; the
  government-architecture companion for the organizational, legal and semantic layers.
  It is guidance, not a technical specification.
- Full glossary, which BB specs **must not redefine**:
  `https://specs.govstack.global/architecture/2-common-terminology`.

