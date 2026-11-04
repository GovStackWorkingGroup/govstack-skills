---
name: govstack-writing
description: Write or edit general GovStack Markdown documentation for an international technical audience, including introductions, reference chapters, how-tos, and evidence-grounded assessments. Use for editorial work and meaning-preserving clarification of existing requirement prose. Route embedded OpenAPI metadata to openapi-writing and GovStack domain reasoning to govstack-concepts. Do not use to change requirement meaning, classifiers, numbering, or grouping, design an API, migrate repository layouts, or manage releases.
---

# GovStack writing

Create clear, exact GovStack documentation for international implementers, architects, and
reviewers. This is an editorial skill. It can clarify existing requirement prose without
changing its meaning. It does not authorize changes to requirement meaning, classifiers,
numbering, or grouping; API behaviour; repository structure; releases; implementations; or
external systems.

Read the [shared writing style](../../shared/writing-style.md) before drafting or editing.
If relative links are unavailable, read
`${CLAUDE_PLUGIN_ROOT}/shared/writing-style.md`. For concrete edits or reviews, also read
[references/examples.md](references/examples.md).

## Route specialized work

- Read `govstack-concepts` before deciding what a GovStack term, actor, or relationship
  means. Verify consequential claims against the owning source rather than treating the
  orientation skill as proof. If related guidance is unavailable, under revision, or
  inconsistent with the owning sources, use those sources and report the gap.
- Use `openapi-writing` for selected `summary` and `description` metadata in an existing
  OpenAPI description. A general Markdown pass preserves fenced OpenAPI examples unless
  editing those examples is also requested. This skill does not design an API.
- Repository structure, identifiers, anchors, versions, publication metadata, and releases
  follow the target repository's instructions and chosen template. A prose pass preserves
  that choice. Legacy chapter layouts and functionality-based layouts both exist; this
  skill does not require migration or treat a draft proposal as an adopted rule.
- Keep requirements engineering separate. Meaning-preserving editorial clarification of an
  existing requirement is in scope. Changing its meaning, classifiers, numbering, or grouping,
  including splitting, merging, moving, or renumbering requirements, belongs to the owning
  project's process.

## Before editing

1. Read the repository instructions, target page, neighbouring pages, and the sources that
   own its factual and normative claims. Record existing dirty files and protect unrelated
   work.
2. Establish the requested boundary: wording-only, broader documentation restructuring, or
   review-only. A request to improve documentation does not authorize running the documented
   procedure, changing external state, or publishing anything.
   For a site, identify the declared publication inputs, such as `toc.yml`, `SUMMARY.md`,
   or build configuration. A wording-only pass preserves that inventory.
3. Select the register:
   - **Overview:** state the reader's technical problem, the document's purpose, and its
     scope early. Write for technical readers working across countries, without expecting
     knowledge of the project's internal organisation.
   - **Reference:** use active present-tense statements with explicit actors. Make each
     established obligation's condition, subject, action, and constraint clear. Where useful,
     give each obligation its own sentence while preserving shared conditions and modal
     scope. Do not split or merge requirements.
   - **How-to:** use imperative, chronological, runnable steps. State prerequisites before
     commands and the observable result after them.
   - **Assessment:** separate observed evidence, interpretation, unresolved gaps, and any
     proposal. Do not present local evidence as deployed behaviour or demonstrated results.

## Preserve meaning

For wording-only edits, preserve requirement levels and normative modals, classifiers,
identifiers, anchors, links, code, commands, schema examples, standard literals, and the
behaviour established by owning sources. Do not silently turn guidance into an obligation,
replace a deliberate term, or normalize spelling across a project. Follow the existing
project's English; retain the exact spelling of protocol names, headers, fields, and other
standard literals.

Use plain, exact English without idioms or unstated local assumptions. Name the actor when
the actor matters, and define uncommon GovStack acronyms on first use. Treat research notes
and draft style rules as proposals unless the owning project has adopted them. This includes
strict sentence caps, forbidden-word lists, blanket replacement of `optional`, and global
spelling rules.

Give rationale, benefit, compliance, and adoption claims only when a source supports them.
When sources conflict or leave behaviour unknown, retain the ambiguity, identify the
evidence gap, and request a decision if it changes the document's meaning. Do not fill the
gap with plausible prose.

## Working notes and publication

Keep task working notes outside committed and published inputs, using the repository's
ignored local working folder when one is configured, or temporary scratch storage. Do not
add a note to navigation or source control merely to support an editorial pass. Flag
existing exposure separately; moving or deleting published material requires the relevant
task scope.

An unlisted document is not automatically private. A public contributor or adoption guide
can remain in the repository without being a compiled specification page. Follow the
declared inventory and the document's purpose instead of publishing every Markdown file or
classifying every unlisted file as a working note.

## Verify and report

Inspect the diff for semantic drift and accidental formatting. Run the repository's
applicable Markdown, link, and documentation checks. If a preview is requested, inspect the
generated pages and navigation against the declared inventory. Re-read changed passages
against their owning sources, paying particular attention to actors, conditions, negation,
modals, classifiers, identifiers, commands, and examples.

Report the files changed, checks and results, unresolved evidence gaps, dirty-tree overlap,
and what the checks cannot establish. A clean documentation build does not prove runtime,
deployment, compliance, or adoption claims.
