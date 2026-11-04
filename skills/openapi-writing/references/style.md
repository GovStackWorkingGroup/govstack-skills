# Style for OpenAPI documentation metadata

Apply the [shared writing style](../../../shared/writing-style.md) for an international
technical audience, then use this reference after identifying the target OpenAPI objects
and their owning behaviour. The shared overview, rationale, and example guidance does not
expand the documentation-field allowlist.

## Source posture

The target repository and version control the contract. Read the OpenAPI Specification for
the version declared by the target description to confirm which objects define `summary`
and `description` fields. The general [OpenAPI Specification index](https://spec.openapis.org/oas/)
is a format reference, not a source of domain behaviour.

[CAMARA API Design Guide Appendix B](https://github.com/camaraproject/Commonalities/blob/main/documentation/CAMARA-API-Design-Guide.md#appendix-b-informative-operationid-and-description-guidelines-for-mcp-and-ai-agent-readiness)
is informative. Its emphasis on self-contained operation prose can improve a review, but its
keywords, templates, verb lists, and agent-readiness goals are not GovStack obligations.
Likewise, a local GovStack API guide marked draft supplies research unless the target
repository explicitly adopts it.

For GovStack vocabulary, read the authoritative source identified by `govstack-concepts`.
Do not copy a glossary into the OpenAPI description or create a competing definition.
Proposed GovStack requirements models are not ratified merely because a draft chapter or
shared skill reference describes them.

Do not turn an editorial pass into a requirements-model migration. There is no global RFC
keyword convention to impose through this skill. Preserve the target document's established
convention and every existing modal's force and scope.

## OpenAPI prose

- Write an operation `summary` as a short action phrase: base-form verb plus the domain
  object and a qualifier only when it distinguishes the operation. For example, "Retrieve
  one registry record". Do not write a slogan, heading fragment such as "Record API", or
  implementation method.
- Use an operation `description` for behaviour a caller needs to choose and use the
  operation correctly: result, relevant condition, side effect, visibility boundary, or
  important distinction from a nearby operation. Do not merely repeat the summary.
- State interface behaviour declaratively, such as "Returns", "Creates", or "Rejects".
  Use the imperative, such as "Omit" or "Send", only for an instruction to the caller.
- Describe schemas and fields by their domain meaning. Include units, reference frame,
  absence or null meaning, default behaviour, or lifecycle meaning when the owning contract
  establishes them and the machine-readable facets do not make the meaning clear.
- Make each independently displayed summary or description understandable without the
  surrounding guide. Expand or avoid unfamiliar GovStack abbreviations where needed;
  do not add a glossary or repeat common protocol names in every field.
- Use `info.description` for relevant, established purpose, scope, and draft or deployment
  context. Keep operation prose focused on the behaviour needed to choose and use it. A
  rationale is useful only when sourced and relevant; it is not required for every field.
- Keep paragraphs and lists readable in generated documentation. Use a YAML literal block
  when line breaks matter, and backticks for field names and protocol literals. Avoid
  renderer-specific formatting.

An [OpenAPI description](https://learn.openapis.org/glossary.html) is the logical API
description, potentially composed from several documents. Use "file" or "document" when
referring to an individual YAML or JSON artifact. If "contract" is ambiguous, distinguish
that artifact from the running interface or its behaviour only when the owning meaning is
clear. Never rename a field, schema, or resource to resolve a prose collision.

Simplify incidental checker or generator wording in public metadata, but retain any
material distinction between structural validation and deployed behaviour. A clean linter
result is not evidence of implementation conformance.

## Vocabulary and actors

Use actors as the owning source defines them. Where the target uses the common distinction
below, keep it intact:

- **caller**: the party or principal whose authority is evaluated, including an anonymous
  caller when the target defines a public operation;
- **client**: the software that sends the request and interprets the response;
- **person**: a natural person represented by, affected by, or interacting with the system;
- **provider, issuer, verifier, registry,** and other named roles: use their exact defined
  meaning and capitalization when the owning domain source defines them.

Do not infer these identities from generic prose or substitute "user" for them unless the
repository defines it unambiguously. A client can act for a caller, and a record can describe
a person, but those facts do not make the three terms interchangeable.

## Meaning that prose edits must preserve

Compare before and after text for every item below that appears in the source:

- normative modals and their strength, including `MUST`, `MUST NOT`, `SHOULD`, `MAY`, and
  lower-case obligation language;
- negation, exceptions, and conditions such as "when", "unless", "only", and "except";
- authorization, disclosure, privacy, redaction, and caller-visible boundaries;
- defaults, omission, nullability, empty values, limits, ranges, units, and ordering;
- temporal coordinates and distinctions such as effective, recorded, current, retained,
  expired, or historical state;
- temporal and authority qualifiers such as current, source-backed, retained, declared,
  only, and no later than. Keep each qualifier explicit or replace it only with wording that
  is demonstrably equivalent. In particular, per-request or per-page authorization under
  current rights or policy must not become a generic claim that authorization occurs;
- cursor opacity, binding to filters and ordering, expiry or reuse limits, and final-page
  meaning;
- side effects, idempotency, retries, asynchronous completion, and error conditions.

Do not weaken an absolute into a tendency, broaden an exception, or turn a condition into an
unconditional claim. Do not add a default, limit, retention period, status code, privacy
claim, or cursor property because it seems conventional.

## Review findings

Raise a finding outside the canonical OpenAPI artifact when:

- the operation, schema, requirement, and implementation evidence disagree;
- an actor or defined term has more than one plausible meaning;
- prose promises behaviour not represented by the contract;
- the contract exposes a constraint or default whose operational meaning is unknown;
- a privacy, temporal, pagination, or error case cannot be stated without a product
  decision.

Give the exact object location, the evidence on each side, and the question an owner must
answer. Suggested text can be conditional on that answer, but it must not be inserted as if
the answer were known.
