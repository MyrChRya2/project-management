# project-management — AI Project Management Agent Skill

| [中文](README.md) | [English](README.en.md) |
| :---: | :---: |

> Distills a "project management session" into a reusable methodology: session-start review → decisions land in docs first → daily wrap-up writes devlog automatically → bug tracking → path changes sync the index → Git commit discipline. Built for **solo indie developers** (games / apps / content), methodology-agnostic, bound to any project via a root `PROJECT_INDEX.md`.

## License

**CC0 1.0 Universal** (public domain dedication) — free to use, modify, redistribute, and use commercially, with no attribution required. See [LICENSE](LICENSE).

## Install

```bash
# Option 1: copy into the user-level skill directory
cp -r project-management ~/.dsh/skills/

# Option 2: git clone into the skill directory
cd ~/.dsh/skills && git clone <repo-url> project-management
```

After install, `project-management` should appear in the available skills list in a new session (restart the session if not).

## Bind a project (first use)

1. Say **"审查进度"** (or "review progress") in a session and provide the **project root directory** (the Git repo root).
2. The skill reads (or generates from template) `PROJECT_INDEX.md` — the project's single configuration entry point.
3. Every session after that starts by reading it; paths and conventions are all resolved from the index.

## Trigger phrases

| You say | What happens |
|---|---|
| "review progress" / session start | Git audit + doc/code consistency check + today's suggestions |
| "today's work is done" / "update the log" | Read template → write today's devlog → sync ROADMAP → commit |
| "log a decision" + content | Decision → devlog + ROADMAP/GDD sync |
| "found a bug" / "fixed a bug" | Register in bug tracker + devlog; update status after fix |
| "sync the index" | Update PROJECT_INDEX.md after path/structure changes |
| "commit the code" | Triage files → conventional commit → push |

## Structure

```
project-management/
├── SKILL.md                        # Master protocol + routing table + 3 principles
├── skills/
│   ├── session-start/SKILL.md      # Session-start review
│   ├── daily-wrap/SKILL.md         # Daily wrap-up devlog
│   ├── decision-log/SKILL.md       # Decision logging & sync
│   ├── bug-track/SKILL.md          # Bug registration & tracking
│   ├── index-sync/SKILL.md         # Index sync
│   └── commit/SKILL.md             # Git commit discipline
├── templates/
│   ├── devlog.template.md          # Devlog template
│   ├── project-index.template.md   # Project index template (= project config)
│   └── decision.template.md        # Major-decision template
├── shared-references/
│   ├── doc-roles.md                # Document role separation
│   └── git-workflow.md             # Git workflow conventions
├── LICENSE                         # CC0 1.0
├── README.md                       # This file (Chinese)
└── README.en.md                    # English version
```

## Adapt to a new project (3 steps)

1. Copy `templates/project-index.template.md` to the project root as `PROJECT_INDEX.md`; replace `{{PLACEHOLDER}}` values.
2. Copy `templates/devlog.template.md` to `docs/devlog/TEMPLATE.md`.
3. Use the default values for `{{DOCS_DIR}}` / `{{ENGINE_DIR}}` (`docs/`, engine dir name) or declare the actual paths in the index.

## Origin

Distilled from the real project-management sessions of **EndlessRegret** (a Godot 2D action-platformer), iterated during live development. Core ideas: **docs are memory, code is the result; commit small and often; focus on one milestone at a time.**
