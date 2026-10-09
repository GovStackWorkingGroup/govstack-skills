# Canonical BB spec structure (the contract)

> **⚠ UNDER REVISION since 2026-09-20 — do not rely on this file.**
> It is being rewritten. Do not cite it, do not apply its rules, and do not use it to
> validate or normalize a spec. Ask the user for the current rules instead.
> Remove this banner when the rewrite lands.


Paths below are relative to the corpus root; see `${CLAUDE_PLUGIN_ROOT}/shared/corpus-map.md`.

Every `bb-*` repo must match this. Pick ONE form per chapter repo-wide — never keep both a
`N-chapter.md` and a `N-chapter/` folder for the same number.

```
spec/
  README.md
  SUMMARY.md                       # GitBook ToC — MUST match the chapter files exactly
  1-version-history(.md OR /)
  2-description.md
  3-terminology.md
  4-key-digital-functionalities.md
  5-cross-cutting-requirements.md
  6-functional-requirements.md
  7-data-structures.md
  8-service-apis.md
  9-workflows.md
  10-other-resources.md
  .gitbook/assets/                 # ALL binary assets (png/pdf/svg) live here
api/                               # OpenAPI/Swagger definitions only (.yaml + .json)
examples/                          # real impls or a `someApp/` placeholder
test/                              # `test/openAPI/` harness or a `plan.md` placeholder
LICENSE                            # filename `LICENSE`, not LICENSE.txt
README.md  .gitbook.yaml  .prettierrc
```

A clean starting point is
`workinggroups-framework/bb-specification-template/bb-govspecs-2-template/`.

Note that chapter 5 is still named `5-cross-cutting-requirements.md` on disk, even though
"cross-functional" is the current term in prose. Don't rename the file to match the prose —
`SUMMARY.md` and the GitBook build depend on the filename.

Always check `SUMMARY.md` matches the actual chapter files after any structural edit — a
mismatch breaks the GitBook build.
