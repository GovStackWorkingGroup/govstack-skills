# Publication metadata and releases

Version-bump rules live in `${CLAUDE_PLUGIN_ROOT}/shared/naming-and-versioning.md`.

## index.yml

A spec's `index.yml` carries the release identity (see the template). Key fields:

- `publication_urn` — e.g. `govstack:bb:cloud-infrastructure:1.0.1`
- `version`
- `version_short_description` — link the PR
- `previous_version`
- `status` — draft → … → release
- `owner_group` — the owning Working Group or team, e.g. `govstack:wg:cloud-infrastructure` or `govstack:team:technical-committee`
- `publish_date`
- `authors` / `editors` — with `govstack:person:<id>` identifiers

## Cutting a release

1. Update `index.yml`: status, version, dates, short description.
2. Set `previous_version`.
3. Update `building-blocks.yml` versions and mark the new one `latest: true`.
4. Confirm `SUMMARY.md` is consistent with the chapter files on disk.

## Where a release sits in the lifecycle

The Specifications Track (new spec or new major version) runs:
Specification Proposal → Specification Draft → Release Candidate → Published Specification
→ Obsoleted Specification.

Minor versions skip the Proposal phase and use an internal WG review. A Proposal must pass a
Product Soundness Review, an Architectural Soundness Review, and Governance Committee
approval, with at least 2 reviewers who are neither credited in the spec nor from an active
member organization. Obsoleting requires an ADR approved by the Architecture Committee and
the Governance Committee.

Authoritative source: `govstack-process/chapters/publication_tracks.md`.
