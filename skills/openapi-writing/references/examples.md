# Illustrative OpenAPI prose examples

These examples illustrate writing choices only. They are not GovStack obligations and do
not establish behaviour for any real API. Each example assumes the stated facts have already
been established by the owning contract or requirements.

## Operation summary and behaviour

Established facts: the operation returns one caller-visible record by identifier and uses
`404` when no visible record matches.

Before:

```yaml
summary: Record retrieval
description: Gets a record.
```

After:

```yaml
summary: Retrieve one registry record
description: >-
  Returns the record visible to the caller for the supplied identifier. Returns `404`
  when no visible record matches.
```

The summary is an action phrase. The description states the result and preserves the
visibility boundary. It does not promise that a record exists merely because the identifier
exists outside the caller's authorization scope.

## Standalone prose for international readers

Established facts: the operation returns the record view visible to the caller for the
supplied identifier. The API description is a draft, and structural validation does not
establish deployed behaviour.

Before:

```yaml
info:
  description: >-
    This section of this draft describes a cutting-edge API. The internal checker validates
    the API description's structure, not deployed behaviour.
paths:
  /records/{recordId}:
    get:
      summary: Get up to speed with a record
      description: Returns the record view visible to the caller for the supplied identifier.
```

After:

```yaml
info:
  description: >-
    This API description is a draft. Structural validation does not establish deployed behaviour.
paths:
  /records/{recordId}:
    get:
      summary: Retrieve a record view by identifier
      description: Returns the record view visible to the caller for the supplied identifier.
```

The revision removes an idiom, marketing, and incidental checker detail. It retains the
draft and verification limits in `info.description`. The operation keeps its own selection
and visibility semantics because it may appear independently in generated documentation.

## Ambiguous words require evidence

Source text:

```yaml
description: >-
  Returns matching Records. A request may use name and/or status. If possible, the
  operation returns all matching Records, etc.
```

The source does not establish how the filters combine or what limits or fallback behaviour
apply. Leave the ambiguous statements unchanged and report those gaps outside the canonical
artifact. Removing "and/or", "if possible", or "etc." without resolving their meaning can
change filtering, completeness, or failure guarantees. Do not infer the answer from a common
API pattern or the need for a cleaner sentence.

## Field meaning, default, and time

Established facts: `effectiveAt` accepts a calendar date or RFC 3339 UTC instant, and
omission selects the currently applicable state.

Before:

```yaml
effectiveAt:
  description: Optional effective date.
```

After:

```yaml
effectiveAt:
  description: >-
    Calendar date or RFC 3339 UTC instant at which the returned record applies. When
    omitted, the operation returns the currently applicable state.
```

"Optional" is not forbidden. Here, replacing it makes the omission behaviour and temporal
coordinate explicit. If the default is not established, keep the omission behaviour out and
raise a question.

## Cursor meaning and limits

Established facts: the cursor is opaque, comes from the preceding page, remains bound to
the original filters and sort order, and is omitted on the first request.

Before:

```yaml
cursor:
  description: Cursor for the next results.
```

After:

```yaml
cursor:
  description: >-
    Opaque continuation token returned by the preceding page. Omit it for the first page.
    The token remains bound to the original filters and sort order.
```

Do not add expiry, single-use, snapshot, or consistency claims unless an owning source
defines them.

## Shared descriptions in generated documentation

Established facts: two list operations reuse a `PageSize` parameter. Its schema is in
`api/common/pagination.schema.yaml`. The renderer displays both the parameter and schema
descriptions. The contract defines a requested upper limit and permits fewer entries.

The parameter already has a short description:

```yaml
description: Requested page limit.
schema:
  $ref: './common/pagination.schema.yaml#/$defs/PageSize'
```

The verbose text comes from the referenced schema:

```yaml
$defs:
  PageSize:
    type: integer
    minimum: 1
    maximum: 100
    description: >-
      The page size is the value which is used to specify the maximum number of entries
      being requested within one page of the results. The server is allowed to return
      a number of entries that is smaller than this requested number.
```

Change only the shared schema's description:

```yaml
description: >-
  Maximum number of entries requested per page. The server may return fewer entries.
```

Record both operations as reuse sites and check the rendered parameter together with the
schema prose. Editing only the parameter would leave the verbose source in place. The
shared description must remain accurate for both operations; an operation-specific default
or guarantee would not belong here. The reference, constraints, and payloads stay unchanged.

## Preserve conditions, negation, and normative force

Established fact: a normative source contains the condition and obligation below.

Before:

```yaml
description: >-
  When `includeHistory` is `false`, the operation MUST NOT return retained revisions.
```

Acceptable clarification:

```yaml
description: >-
  When `includeHistory` is `false`, the response MUST NOT contain retained revisions.
```

The edit keeps `when`, `false`, `MUST NOT`, and the distinction between retained revisions
and the current record. Rewriting it as "Usually returns only the current record" would
weaken all four.

## Preserve current authorization qualifiers

Before:

```yaml
description: Each page applies current authorization.
```

Acceptable:

```yaml
description: The operation evaluates current authorization for every page.
```

Unsafe:

```yaml
description: Authorization is applied to each page.
```

The unsafe wording keeps the per-page scope but drops `current`, so it no longer says that
rights or policy are evaluated as they stand for that page. Paraphrase is fine when every
qualifier remains explicit or the replacement is demonstrably equivalent.

## Explain enum meanings without changing the enum

Established facts: the three values have the meanings shown.

Before:

```yaml
status:
  type: string
  enum: [ACTIVE, SUSPENDED, CLOSED]
  description: Registration status.
```

After:

```yaml
status:
  type: string
  enum: [ACTIVE, SUSPENDED, CLOSED]
  description: |
    Registration lifecycle state:
    - `ACTIVE`: available for ordinary processing
    - `SUSPENDED`: retained but temporarily unavailable for ordinary processing
    - `CLOSED`: no longer available for ordinary processing
```

Only `description` changes. If "ordinary processing" is not a defined domain concept, use
the repository's established term or leave the meanings unresolved.

## Distinguish caller, client, and person

Established facts: authorization is evaluated for a caller, client software submits the
request, and returned records describe people.

Before:

```yaml
description: Returns user records the client can access.
```

After:

```yaml
description: >-
  Returns records about people that the authenticated caller is authorized to read. The
  client submits the request on the caller's behalf.
```

Do not add the second sentence when the client and caller relationship is not established.

## A similarly named key that is out of scope

The inner keys below are example payload data, not OpenAPI documentation metadata. A prose
pass must leave them unchanged.

```yaml
example:
  report:
    summary: Daily intake
    description: Text supplied by the report author
```

The same rule applies to arbitrary data nested in extensions. Confirm the containing
OpenAPI object before editing any key named `summary` or `description`.

## Leave a contradiction unresolved

Suppose an operation description says "Returns every retained revision", its response
schema permits only one current record, and a requirement says retained revisions are
available only through a separate history operation. None of these editorial rewrites is
safe:

- changing "every" to "the current" assumes the schema is authoritative;
- widening the schema changes the API contract;
- deleting the sentence hides a possible missing behaviour.

Leave the canonical OpenAPI description unchanged. Report the operation's JSON Pointer,
the three conflicting sources, and ask whether the operation returns the current record or
retained revisions. Draft alternative wording for each answer only if it helps the owner
decide.
