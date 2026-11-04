# Writing for an international technical audience

Apply these editorial conventions to GovStack documentation. They do not establish API
behaviour, requirement strength, conformance, or publication status. The target repository's
owning sources and explicit instructions govern those matters.

## Reader and vocabulary

Write for implementers, architects, integrators, and reviewers working across countries.
Assume technical competence, but do not assume English is the reader's first language or
that the reader knows GovStack's internal vocabulary or one country's public administration.

- Prefer familiar, concrete words and verbs when they retain the exact meaning. Use "use"
  rather than "utilise" and "to" rather than "in order to". Keep precise technical terms:
  authentication and authorization, for example, must not collapse into a vague "access check".
- Avoid idioms, jokes, cultural metaphors, and regional shorthand. Replace "get up to speed"
  with the actual task or knowledge needed. Do not turn an idiom into an unsupported promise
  such as automatic integration or operation without configuration.
- Expand unfamiliar GovStack and domain abbreviations at their first useful occurrence in a
  self-contained page or description, or avoid the abbreviation. Do not repeatedly expand
  common protocol names where the target audience already understands them.
- Name the actor and the object. Replace ambiguous "it", "they", "the system", or "user"
  only when the source establishes the intended referent. A person, client application,
  caller, and implementation are not interchangeable.
- Keep jurisdiction, institutional roles, eligibility, and deployment assumptions as narrow
  as the source makes them. Do not generalise one country's process into a GovStack rule.
- Make explanatory dates, units, and time zones unambiguous when their meaning is known.
  Preserve machine values and source examples; do not reinterpret an ambiguous date or
  convert a declared unit, limit, or time coordinate through an editorial change.

Use the target repository's declared English variant, or follow the surrounding document
where no policy is declared. Do not normalise British and US spelling across existing text
as part of an ordinary prose pass. Protocol literals, identifiers, standard titles, and
standard-defined terms retain their established spelling, including `Authorization`.

## Choose the register by the reader's task

| Register | Reader's need | Shape and voice |
| --- | --- | --- |
| Overview | Understand the technical problem, purpose, applicability, and scope | Lead with the problem or outcome and who needs it. Use plain third-person statements. Explain what the document or interface changes using established facts. |
| Reference | Implement, integrate, or assess a defined interface or rule | Use neutral, active, present-tense statements. Make the condition, subject, action, object, and relevant constraint clear. |
| How-to | Complete one task | Use imperative steps in the order performed. Retain prerequisites, working-directory context, exact commands, expected results, and relevant failure handling. |

Choose the register for the passage, not just its filename. An overview can introduce a
reference page, and an operation description can include an instruction to the caller.
Reference prose states behaviour; instructions tell a named reader what to do.

State what is in scope. For excluded concerns, identify the owning component, document, or
process when the source establishes it. Do not invent an owner or a link to complete the
pattern. Keep overviews useful to a technical reader; do not add a funding pitch or a
benefits narrative to every reference entry.

## Exact terms and established meaning

Read `govstack-concepts` and the owning terminology source when a domain term matters.
Link to the source instead of copying or redefining its glossary. Preserve one term per
concept and the source's defined capitalization.

Use "Building Block Specification" and "Building Block Software" where a bare "Building
Block" would blur the document and its implementation. Distinguish a document or API
description, the running interface, and its behaviour. If "contract" could mean any of those
or a procurement instrument, choose the precise noun when the referent is established.
An explicitly defined "API contract" can remain exact; this is not a find-and-replace rule.

For an HTTP API, use "operation" for a method-plus-path and "endpoint" for a route or URI
when the owning terminology makes that distinction. Preserve established terms such as
"Operation resource" and report a collision rather than renaming resources or identifiers.

Treat vague phrases as questions about meaning. "And/or", "etc.", "if possible", "as
appropriate", and unqualified "all", "always", or "never" can hide choices or guarantees.
Clarify them only from owning evidence. Do not silently choose AND versus OR, remove an
exception, or replace an unresolved claim with a plausible default. Exact universal
requirements, schema optionality, and defined uses of "support" remain valid.

When editing normative prose, preserve its convention, modals, classifiers, actors,
conditions, exceptions, and identifiers. Where useful, give each obligation its own sentence
without losing a shared condition or modal. This is not permission to split or renumber
requirements, move obligations, add RFC keywords, or migrate a requirements model.

## Readability and punctuation

- Use sentence case for headings and summaries. Preserve stable anchors, link targets,
  defined names, and code literals even if their casing differs.
- Prefer concrete verbs to noun-heavy phrases and introductory boilerplate. Remove
  "This section provides context" when the following text supplies that context directly.
- Remove promotional claims such as "seamless", "powerful", or "game changer" when they
  carry no established technical meaning.
- Use the serial comma where it clarifies a list. Do not use em dashes.
- Separate dense conditions, consequences, or alternatives into paragraphs, lists, or
  tables when that improves comprehension. There is no mandatory word count, sentence cap,
  list length, or cross-reference quota. Preserve relationships between conditions.
- Use descriptive links to stable sources. Explain an important condition locally when
  the passage must stand alone; a link is not a substitute for the information needed to
  choose or use an operation.

Remove unnecessary self-reference such as "this section", "the book", or "this revision".
Keep draft warnings, versions, applicability, and limitations when they affect how the
reader can use the material. A cleaner opening must not imply publication or maturity that
the source does not establish.

## Reasons, examples, and verification

Include a short rationale when an owning source establishes it and it helps the reader
understand a non-obvious rule. State the concrete consequence and affected party. Do not
invent savings, risks, guarantees, legal effects, or runtime behaviour to supply a "why"
sentence. A rationale is not a new obligation.

Use small examples when they resolve a structural ambiguity, and label illustrations as
illustrations. Do not invent normative values or expected outcomes. A writing pass preserves
existing code, commands, API example payloads, and schemas unless changes to them are also
authorized. Use `openapi-writing` for selected documentation metadata in an existing OpenAPI
description. A general Markdown pass preserves fenced OpenAPI examples unless editing those
examples is also requested.

Explain verification in reader terms: what structural validation checks, what a reviewer
checks, and what needs implementation tests or operational evidence. Do not copy checker
function names, generator notes, or internal paths into reference prose merely because a
tool emits them. Tool names, paths, and commands needed to perform a documented task belong
in the how-to. Keep material verification limits even when removing implementation detail.

Distinguish a proposed rule, an adopted requirement, a validated artifact, and demonstrated
implementation behaviour. Do not promote one into another through wording. When sources
conflict or leave meaning unresolved, retain that uncertainty and report the exact gap
outside the canonical artifact instead of supplying an answer.
