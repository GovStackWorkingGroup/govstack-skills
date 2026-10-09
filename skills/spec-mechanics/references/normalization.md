# Normalizing a non-conforming BB repo

Paths below are relative to the corpus root; see `${CLAUDE_PLUGIN_ROOT}/shared/corpus-map.md`.

Detail: `workinggroups-framework/bb-specification-template/STRUCTURE-ANALYSIS.md` and
`NORMALIZATION-CHECKLIST.md`.

## Order of operations

The order matters — deleting before merging loses content, and rebuilding `SUMMARY.md`
before the files settle produces a broken ToC.

1. Diff the repo against the canonical structure (`canonical-structure.md`).
2. **Merge** unique content into the canonical file.
3. **Then** delete the obsolete file.
4. Move all binary assets into `spec/.gitbook/assets/`.
5. Rename chapters to canon.
6. **Rebuild `SUMMARY.md`** to reference only surviving files.

## Common outliers to watch for

- Off-by-one chapter numbering
- `8-apis-and-services.md` vs canonical `8-service-apis.md`
- `9-internal-workflows` vs canonical `9-workflows`
- Extra chapters (`11-key-decision-log`, `12-future-consideration`) that should fold into
  `10-other-resources.md`
- Both a `N-chapter.md` and a `N-chapter/` folder for the same number — pick one form
  repo-wide
- `LICENSE.txt` instead of `LICENSE`
