---
name: openapi-writing
description: Write or review human-readable OpenAPI summary and description metadata without changing API behaviour or structure. Use for operation, schema, parameter, response, and field prose in an existing OpenAPI description, including referenced documentation. For GovStack domain meaning use govstack-concepts. Do not use this skill to design or implement an API, change identifiers or schemas, write GovStack requirements, migrate repository layouts, or manage releases.
---

# OpenAPI writing

Improve prose that helps a reader understand an existing OpenAPI description. Preserve the
API contract. This skill does not authorize API design, implementation, requirement, or
release changes.

Read `govstack-concepts` before deciding what a GovStack term or role means. Verify its
guidance against the target repository's owning sources. If a related skill is unavailable,
marked under revision, or inconsistent with those sources, use the owning sources and
report the gap instead of applying stale guidance. Preserve the repository's chosen layout
and publication inventory; an editorial pass does not migrate its template or release model.

## Before editing

1. Record the current HEAD, branch, and dirty files. Treat existing changes as someone
   else's work and identify any overlap with the requested metadata nodes.
2. Locate the canonical OpenAPI description and read its declared OpenAPI version,
   repository instructions, nearby operations and schemas, and the requirements or domain
   sources that own the behaviour being described.
3. Trace the text under review to its source objects, including local and external `$ref`s,
   composition, and descriptions on both a parameter and its schema. A renderer can show
   both descriptions. Record the source file, object location, and known operations or
   schemas that reuse a shared description before deciding what to edit.
4. Define an allowlist of exact object locations to edit, preferably as JSON Pointers. Each
   target must be a `summary` or `description` field defined as documentation metadata on
   an OpenAPI object or a JSON Schema object in the declared version and dialect.
5. Read the [shared writing style](../../shared/writing-style.md) and
   [references/style.md](references/style.md). For an edit or a concrete review, also read
   [references/examples.md](references/examples.md). Resolve bundled links against the file
   containing the link, not the working directory.

Do not treat the draft GovStack API design guide, research notes, CAMARA guidance, or the
latest OpenAPI version as authority over the target repository and declared OpenAPI version.

## Writing boundary

Change only allowlisted documentation scalar values. Never change:

- paths, methods, names, identifiers, references, schema shape, types, formats, constraints,
  required fields, enum values, defaults, example values or payloads, security declarations,
  Security Scheme fields other than an explicitly allowlisted `description`, extensions, or
  ordering;
- a `summary` or `description` key inside example data, extension payloads, or other
  user-defined data merely because its name looks like OpenAPI metadata;
- API behaviour, implementation, tests, separately authored requirements, or classifiers.

For an allowlisted Security Scheme Object `description`, verify every claim against the
owning security sources. Treat it with the same semantic caution as operation prose. Never
change a Security Requirement Object, scheme name or type, OAuth flow, scope, protocol
value, or other security declaration.

Do not apply a repository-wide text replacement. Make surgical, object-aware edits. A key's
meaning comes from its OpenAPI object location, not from its spelling.

Referenced prose is part of the logical API description, but its ownership and the requested
scope still apply. Check a shared wording change against every known reuse site. If one
operation needs a different meaning, keep the shared text unchanged and report the conflict.
Do not edit a vendored schema or another project's source incidentally. Keep review notes in
local working storage, outside committed or published API inputs.

If canonical sources conflict or do not establish a meaning, leave the canonical artifact
unchanged at that point. Report the location, the conflicting or missing evidence, and the
decision needed. Do not write a plausible behaviour into the API description.

## Verification

After editing:

1. Inspect an object-aware diff and confirm that only allowlisted scalar values changed.
   Where tooling permits, compare parsed before and after documents with those values
   masked. Also inspect the ordinary diff for accidental reformatting.
2. Run the repository's existing YAML, OpenAPI, lint, and documentation validators that
   cover the changed artifact, using repository commands and pinned dependencies. Do not add
   or replace validators or rulesets merely for this prose pass.
3. Review every changed sentence against its owning operation, schema, requirements, and
   vocabulary. Check especially modals, negation, conditions, actors, privacy, defaults,
   limits, temporal meaning, and cursor meaning.
4. When generated documentation is part of the task, use the repository's existing renderer
   to check the affected passages and shared reuse sites. Check parameter and schema prose
   together for repetition. If rendering cannot be checked, report that limit; do not claim
   that a source edit fixed the displayed page.
5. Report changed files and nodes, shared reuse affected, checks and results, unresolved
   contradictions, dirty-tree overlap, and what the checks cannot establish. Passing syntax
   validation does not prove that the prose describes deployed behaviour.

For review-only requests, report findings and proposed text without editing unless the
request also authorizes changes.
