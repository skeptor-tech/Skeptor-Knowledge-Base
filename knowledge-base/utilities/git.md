# Git Utilities

## Overview

Git offers rich tooling for exploring a repository's history so you can keep contributors informed about the current state of the project. The `git rev-list` plumbing command powers many history-aware workflows by returning the commits that match the filters you specify.

## Key capability: `git rev-list`

- Enumerate commits that touch the codebase, optionally constrained to branches, tags, paths, or date ranges.
- Combine with filters such as `--author`, `--since`, `--until`, `--grep`, or `--no-merges` to surface the changes stakeholders care about right now.
- Pipe the resulting commit IDs into `git show`, `git diff`, or `git log --stdin` to inspect code, summaries, or statistics tied to each revision.
- Add `--count` for quick status updates (“8 commits since yesterday on main”) when keeping collaborators informed.

## Usage snippets

Default history walk for the current branch:

```bash
git rev-list --max-count=20 HEAD
```

Targeted walk to highlight a teammate's recent work on documentation paths:

```bash
git rev-list --since="2 days ago" --author="A. Teammate" -- "docs/"
```

Each variant produces a list of commit hashes. Feed those hashes into other Git commands or dashboards to answer specific questions about ongoing work.
