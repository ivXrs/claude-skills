# claude-skills

A personal collection of [Claude Code](https://claude.com/claude-code) skills. Each skill is a self-contained folder with a `SKILL.md` that Claude loads when the task matches its description.

## Skills

| Skill | Description |
| --- | --- |
| [`git-workflow`](git-workflow/SKILL.md) | Git delivery workflow: typed branches, selective staging, atomic Conventional Commits, push, GitHub PRs via `gh`, squash merge, and cleanup. |

## Installation

Clone the repository:

```bash
git clone git@github.com:ivXrs/claude-skills.git
```

### Global (all projects)

Skills in `~/.claude/skills/` are available in every project. Link a skill so that `git pull` keeps it up to date.

Windows (junction, no admin required):

```bat
mklink /J "%USERPROFILE%\.claude\skills\git-workflow" "<path-to-clone>\git-workflow"
```

macOS / Linux:

```bash
ln -s "<path-to-clone>/git-workflow" ~/.claude/skills/git-workflow
```

Alternatively, copy the skill folder into `~/.claude/skills/`.

### Per project

Copy or link the skill folder into the project's `.claude/skills/` directory.

## Structure

```text
claude-skills/
├── LICENSE.txt
├── README.md
└── <skill-name>/
    └── SKILL.md
```

## Requirements

`git-workflow` expects `git` and the authenticated [GitHub CLI](https://cli.github.com/) (`gh`).

## License

Released under the [MIT License](LICENSE.txt).
