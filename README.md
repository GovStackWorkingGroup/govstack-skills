# govstack-skills

A Claude Code plugin holding the skills for working on GovStack specifications.

## Layout

```
.claude-plugin/
  plugin.json                       # plugin identity
  marketplace.json                  # lets this repo be installed as a local marketplace
skills/
  govstack-concepts/                # knowledge: the domain model. Read before reasoning.
  spec-mechanics/                   # procedure: repo layout, normalization, releases
shared/
  corpus-map.md                     # where the GovStack corpus lives; which copies are stale
  requirements-model.md             # FR/CFR, identifiers, the three classifier dimensions
  naming-and-versioning.md          # govstack-[type]-[name], URNs, semver, extension rules
```

## Install

```sh
/plugin marketplace add ~/Documents/programming/govstack/govstack-skills
/plugin install govstack@govstack-skills
```

Updating is `git pull` — no copying into `~/.claude/skills/`. Hand-copied skills drift:
the previous `~/.claude/skills/govstack-concepts/` had five path fixes that never made it
back to this repo.

## Conventions for new skills

**One skill per job, named for the job.** Descriptions are the routing table — Claude picks
a skill by matching the request against the `description` field. Two skills whose
descriptions overlap will shadow each other, and the broader one usually wins. A skill named
`govstack` covering "authoring, reviewing, normalizing, releasing" would swallow every
request meant for `requirements-validator` or `specification-writer`; that is why the old
root skill became the narrower `spec-mechanics`.

Write each description to say what the skill is for **and what it is not for**, with an
explicit redirect. See the two existing skills for the pattern.

**Knowledge vs. procedure.** `govstack-concepts` explains what things mean and is read
before reasoning about GovStack content. Everything else does a job. Procedural skills must
not re-explain the domain — say "read `govstack-concepts` first" and move on.

**Shared material goes in `shared/`.** Anything two skills both need — the classifier table,
the naming schema, corpus locations — lives in one file there and is referenced as
`${CLAUDE_PLUGIN_ROOT}/shared/<file>.md`. That variable resolves regardless of cwd. Never
copy a table between skills; the copies diverge.

**Keep SKILL.md thin.** A SKILL.md is loaded in full when the skill triggers. It should
carry the routing logic, the workflows, and the rules that are dangerous to get wrong —
then point at `references/` for the detail. Lookup material (glossaries, source tables,
checklists) belongs in `references/`, which is read only when needed.

**Never use bare relative corpus paths.** `cfr-architecture/2-common-terminology.md`
resolves only if the cwd happens to be the corpus root. Record the location in
`shared/corpus-map.md` and point at it. Several repos were re-cut in September 2026 and the
obvious paths now hit stale copies.

### Planned

`requirements-validator`, `specification-writer`, `procurement-writer`. All three will lean
on `shared/requirements-model.md` rather than restating it.
