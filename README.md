# dartrverse-skills

Claude Code skills for the dartRverse development team.

## What this is

The dartR team uses Claude Code to review and improve the 200+ functions
of the dartRverse packages. This repository holds the shared skills that
standardise that work, so every team member's Claude session follows the
same procedure and produces comparable, auditable results.

Current skills:

| Skill | Purpose |
|---|---|
| `dartr-function-review` (v1.3.1) | Review-and-improve workflow for one dartRverse function: advisory review → member approval → apply → PR |

## How the function review works

One invocation covers one function, with the human in control at three
points:

1. **Review (read-only).** The skill claims the function in the campaign
   manifest, snapshots its current behaviour as a characterization test,
   and reviews it on two axes — is it built right (conventions) and is it
   the right thing (behaviour vs documentation) — citing numbered rules
   from the conventions catalogue.
2. **On-screen summary.** You get the verdicts, a findings table
   (issue / what happens / proposed fix / severity), the path of the full
   report, and clickable approval boxes. Nothing is changed yet.
3. **You approve specific changes.** Click boxes or reply in free text
   ("apply 1 and 3"). Changes that alter behaviour or an API are isolated
   so you approve the consequence explicitly.
4. **Apply and verify.** Only approved changes are implemented; the
   characterization test proves nothing else moved. You see the commit
   message before anything is pushed.
5. **One PR per function**, linking the full review report.

The skill enforces the team's coding conventions
(`skills/dartr-function-review/references/dartr-conventions.md`) —
extracted from "For the Developer" (Georges, Gruber & Mijangos 2021) and
modernised against the current codebase. Rules marked `[proposed]` await
team ratification and are reported but not enforced.

## Install

Requires [Claude Code](https://claude.com/claude-code). Pick one:

**Personal install (recommended)** — available in every project:

```bash
git clone https://github.com/mijangos81/dartrverse-skills.git
ln -s "$(pwd)/dartrverse-skills/skills/dartr-function-review" ~/.claude/skills/
```

(The repository is private and will be transferred to `green-striped-gecko`
once org access is arranged; the clone URL will redirect after transfer.)

Update later with `git pull` — the symlink picks up changes automatically.

**Per-project install** — copy into a package checkout:

```bash
cp -r dartrverse-skills/skills/dartr-function-review /path/to/dartR.base/.claude/skills/
```

Verify: start Claude Code and ask `review gl.report.callrate` — the
`dartr-function-review` skill should announce itself.

## Use

In a Claude Code session inside a dartR.* package checkout:

- "review gl.report.callrate"
- "run the function review on gl.impute"
- "next function in the manifest"

Campaign state lives in the package repository, not here:
`function-review/manifest.csv` (who has claimed what) and
`function-review/reports/<package>/<function>.md` (the full reports).

## Repository layout

```
skills/dartr-function-review/
  SKILL.md                       the workflow (three phases, gates)
  references/
    dartr-conventions.md         48 numbered rules the review cites (FS1, DAT2, ...)
    report-contract.md           report structure every review follows
    pr-template.md               MR title/body structure
    style.md                     prose style for reports, chat, and MRs
    fixtures.md                  known historical bugs the skill must keep finding
```

## Contributing

- Edit on a branch, PR to `main`.
- Bump `version:` in `SKILL.md` in the same commit that changes behaviour
  (MAJOR: workflow or report contract change; MINOR: new checks or steps;
  PATCH: wording).
- After any edit to the skill or the conventions catalogue, re-run the
  review on one fixture from `references/fixtures.md` and confirm the
  listed defect is still found — a missed fixture is a regression in the
  skill.
- Rule changes in `dartr-conventions.md`: `[confirmed]` requires evidence
  from the current codebase; anything else enters as `[proposed]` until
  the team ratifies it.

## Team

dartRverse: https://github.com/green-striped-gecko/dartRverse
Questions: https://groups.google.com/d/forum/dartr
