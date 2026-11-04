# GovStack writing examples

These examples show editorial transformations, not new GovStack requirements or evidence.
Verify every factual or normative statement against the target repository's owning sources.

## Overview register

**Established facts**

- The intended readers are implementers who need to determine which shared interface rules
  apply to Building Block Specifications.
- The page states the shared rules, their applicability, and the decisions left to each
  specification.
- Service policy and internal software architecture are outside the page's scope. The source
  does not identify their owning documents.

**Before**

> This page gives implementers an overview of shared interface rules for Building Block
> Specifications. It describes where the rules apply and which decisions each specification
> retains. It does not cover service policy or internal software architecture.

**After**

> Implementers need to determine which shared interface rules apply to a Building Block
> Specification. This page states those rules, their applicability, and the decisions left
> to each specification. Service policy and internal software architecture are outside its
> scope.

The revision opens with the reader's problem, purpose, and scope. It does not assume that an
international reader knows the project's internal structure, invent an owner for excluded
concerns, or use promotional adjectives.

## Reference register

**Before**

> When version checks apply, a client request **MUST** include the identifier and **SHOULD**
> include the version. The receiving system validates the version before processing.

**After**

> When version checks apply, a client request **MUST** include the identifier. Under the same
> condition, the request **SHOULD** include the version. The receiving system validates the
> version before it processes the request.

The revision preserves the actors, condition, sequence, and original `MUST` and `SHOULD`
levels while separating the obligations. If the source does not establish one of those
elements, leave that point unresolved instead of adopting this example's wording.

## How-to register

**Before**

> From the repository root, run `npm run check`. If the command reports documentation
> errors, resolve them and run the command again. Repeat until the command exits
> successfully, and continue only after that result. A successful result checks the
> documentation structure; it does not test runtime behaviour.

**After**

> 1. From the repository root, run:
>
>    ```sh
>    npm run check
>    ```
>
> 2. If the command reports documentation errors, resolve them and run `npm run check` again.
>    Repeat this step until the command exits successfully.
> 3. Continue only after the command exits successfully.
>
> A successful result checks the documentation structure. It does not test runtime
> behaviour.

The revision preserves the working directory, command, repeat condition, error handling,
stopping criterion, and verification limit while presenting them as chronological steps.

## Retain ambiguity and unknowns

**Source evidence**

> The record can have the state `pending` or `active`.

**Do not invent**

> The system changes a record from `pending` to `active` after an administrator approves it.

**Write or report instead**

> The source defines the `pending` and `active` states, but it does not define who changes
> the state or what triggers the change.

The last version preserves what is known and makes the evidence gap visible. Add reference
behaviour only after an owning source defines it. Tests can support a separately labelled
observation about an implementation; passing a test does not make that behaviour normative.

## Protect commands, code, and standard literals

**Illustrative source text**

> Include the `Authorization: Bearer <token>` header. Validate the API description with
> `example-validator validate api.yaml`.

**Safe wording edit**

> Include the `Authorization: Bearer <token>` header.
>
> To validate the API description, run:
>
> ```sh
> example-validator validate api.yaml
> ```

Do not translate, respell, or otherwise improve a header name, field, identifier, example
value, or command while editing the surrounding prose. If the literal appears wrong, report
it as a separate possible contract or mechanics issue. `example-validator` is a fictional
name used only for this illustration; the example does not assert that a GovStack CLI exists.

## Keep the chosen format and publication boundary

Suppose a repository publishes selected introduction and functionality files through
`toc.yml`. Its public `CONTRIBUTING.md` is repository-only, and `.local/review-notes.md`
is already ignored working material. These are illustrative repository choices, not a
required GovStack layout.

A wording-only edit improves the requested passages in their existing files. It preserves
the inventory, requirement metadata, anchors, links, and fenced examples. It leaves the
contributor guide repository-only and the review notes local. Adding either document to
navigation would change publication scope; moving the text into ten numbered chapters
would change the chosen format. The same editorial boundary applies when a repository
already uses the chapter layout.
