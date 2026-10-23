# AGENTS

Example apps for Cairn, copied from the main repo (`lean/Examples/`, `host/Sources/Cairn<App>/`)
with the same paths. They're published to read, not to build here: the framework itself is not
public yet. Change an example in the main repo first, then copy it over.

## Workflow: lane

Every change happens in a lane, never in the main checkout and never with a bare
`git worktree add`.

1. `lane new <name>` from an up-to-date main, then work in the printed tree.
2. `lane why <path>` before editing a file.
3. Commit as you go. Add a `Why: <path> | <note>` trailer only for a non-obvious constraint.
4. `lane check`; confirm, replace or retire stale notes.
5. `lane push`, then `gh pr create`.
6. Squash-merge, `git pull --ff-only` in the main checkout, `lane prune --dry-run`, then
   `lane rm <name>` for merged lanes prune misses.

<!-- lane:protocol -->
## Context memory

- Before editing a file, read `.lane/memory/<path>/` if it exists, or run `lane why <path>`.
- Record non-obvious findings with `lane note add <path> -a <anchor> "..."`.
- Do not edit `.lane/` by hand; landing manages it.
- Land with `lane merge`, or `lane push` where trunk is protected, then `lane prune` once it merges.
- Detailed workflow lives in `.agents/skills/lane/SKILL.md`; run `lane install skill` if it is absent.
<!-- /lane:protocol -->
