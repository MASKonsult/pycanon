<!-- Copy of repository_standard.md from MASKonsult/repository-index, commit 958f011, version 20261024a. Refreshed from the source; do not edit here. -->

# Repository standard

This file is the standard for how a repository is laid out and worked in. It is
the single source: every repository carries a copy as `00-Control/standard.md`,
refreshed from here and never edited in place. How a repository is built and who
has access to it is in `build_steps.md`, which management reads and a repository
user does not need.

## Directory convention

Standard version: `20261024a` (`YYYYMMDD` followed by a lowercase letter: `a` for the
first version on a date, then `b`, `c`; raised whenever this standard changes, so the latest
version sorts last).

Source: Jira OPS-186, which owns the numbering. The same stage numbers apply to
Git repositories and to Drive, cloud and filesystem workflows. Use the numbered
directories the project has material for; the gaps are intentional, so do not
create empty stages or reorganise existing repositories solely to match this
convention.

| Number | Directory | Purpose |
| --- | --- | --- |
| 00-09 | `00-Control` | Controls, instructions and process governance: repository instructions, decisions, conventions, configuration documentation. |
| 10 | `10-Inputs` | Inputs and retained source material: supplied sources, imports, exports, source references. A repository with a `10-Inputs` directory records its sources in `10-Inputs/sources.md` (where each came from, where it sits, whether it syncs). Everything in `10-Inputs/` syncs through Git except `10-Inputs/local-only/` (archives, media, large originals; registered with size and SHA-256). There are no default sources shared by all repositories: each repository registers its own. A shared second brain on Drive could supply defaults later; that is not decided. |
| 20 | (no fixed name) | Optional stage between inputs and design. The name depends on the process. |
| 30 | `30-Design` | Requirements, exploration, analysis, architecture, schemas, interfaces. Add subdivisions only when the work calls for them. |
| 40 | (no fixed name) | Optional stage between design and implementation. |
| 50 | `50-Implementation` | Development: code, scripts, manifests, tests, validation, implementation documentation. Conventional internal names such as `src`, `tests`, `scripts` and `config` stay unnumbered. |
| 60 | (no fixed name) | Optional stage between implementation and outputs. |
| 70 | `70-Outputs` | Generated deliverables of the process. |
| 80 | `80-Operations` | Operations and process-learning records. |
| 85 | `85-Reporting` | Reporting and execution evidence, including weekly as-built reports that feed sprint review and planning. Whether to retain or retire this stage is undecided; do not retire it through this convention. |
| 91-98 | `91-<Reviewer>`, `92-<Reviewer>`... | Adversarial reviews, one agent reviewing another's work. Allocated per repository in order of reviewer introduction, whichever agent owns the project: Claude-owned with Codex first gives `91-Codex`; Codex-owned with Claude Code first gives `91-Claude-Code`. Each reviewer writes in its own directory. A new reviewer takes the next free number. If the range runs out the scheme is revisited; decimal suffixes were discussed and not agreed. `90` is not used. One primary owner and one reviewer at a time is the working pattern. |
| 99 | `99-Archive`, `99-junk`... | Dead ends, obsolete approaches and material kept on disk only for recovery or history. Never tracked: every repository's `.gitignore` ignores `99-*/`. Anything useful belongs in another directory instead. |

Components of a standard repository. These control files are what
`layout_standard` in the index refers to:

- `README.md`
- `AGENTS.md` - entry point for agents, pointing at the README and
  `00-Control/standard.md`
- `CLAUDE.md` - one line importing `AGENTS.md`
- `00-Control/standard.md` - copy of this file, with its version in the header
- `00-Control/deployment.md` - the deployment record
- `.gitignore` containing the `99-*/` rule (and `10-Inputs/local-only/` when that
  directory exists)

`10-Inputs/sources.md` is part of the standard only where the repository has a
`10-Inputs` directory. Other numbered directories exist only when the project has
content for them. `00-Control/provisioning-record.md` is written when a repository
is provisioned.

Feedback paths between stages:

- `70` to `10`: one process's or repository's outputs become another's inputs.
  Keep the source references across the handoff.
- `80` to `00`: operational and process experience updates the next set of
  controls and instructions.
- `85` can sit alongside running work and feeds weekly and sprint review.

Rules for specific places:

- Repository-root files such as `README.md` and `.gitignore` keep their
  conventional locations.
- New repositories keep their deployment record at `00-Control/deployment.md`;
  legacy repositories may use `docs/deployment.md`. Deployment runbooks under
  `80-Operations/deployment/` were discussed on OPS-271 and are not agreed;
  inspect existing placement before moving anything.
- The earlier `50-Build` and `99-Trash` names are retired in favour of
  `50-Implementation` and `99-Archive`.
- Drive folders: scope or container folders can stay unnumbered, and numbered
  folders are named `NN-Title-Case-With-Dashes`. Pick the stages the workflow
  needs and descriptive names for them, keeping the numeric meanings above. The
  `20`, `40` and `60` slots have no universal names. The `91`-`98` reviewer
  allocation is specific to Git repositories.

## Working in a repository

- **Remote:** `origin`, over SSH (`git@github.com:<owner>/<repo>.git`). Push
  identity comes from the SSH key, not from the `gh` CLI's active account.
- **Branches:** `main` is the default branch. Work on a task branch and push it;
  Matt merges to `main`.
- **Commits:** manual, at logical checkpoints. Commit messages describe *why*,
  not *what*. Never add Co-Authored-By or "authored by" attribution. Agent-made
  commits end with an `Agent: <agent>` trailer using one of the agent names below
  (for example `Agent: claude-code`), per OPS-271. Further commit labelling is
  still to be decided.
- **Commit author:** `Matt Sheumack <189762347+MattSheumack@users.noreply.github.com>`, GitHub's noreply address for the
  `MattSheumack` account (Matt, 22 Oct 2026), so no real email address goes into commits. Git takes the
  author from configuration and the machine's global author is a placeholder, so give it on each commit
  (`git -c user.name="Matt Sheumack" -c user.email=189762347+MattSheumack@users.noreply.github.com commit`). Earlier commits carry
  `matt@maskonsult.com` or `matt@sheumack.com`; history is append-only and stays as it is.
- **Agent names (Matt, 20 Oct 2026; `spark` added 23 Oct 2026):** `claude-code`,
  `claude-cloud`, `claude-ai`, `chatgpt`, `codex`, `codex-cloud`, `agy`, `gemini`,
  `jules`, `spark`. One name per
  execution surface, used the same way everywhere: the `Agent:` commit trailer;
  the plus tag in email to `mattsheumack+<agent>@gmail.com`; the tag in Slack
  conversations; and the Jira and Confluence label `agent-<agent>`.
- **History is append-only:** no `reset --hard`, `rebase`, `push --force`,
  `filter-branch`, or `restore`.
- **Secrets:** credentials, session profiles and retrieved media never go in Git.
