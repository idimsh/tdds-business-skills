# Agent Skills

Portable SKILL.md-based agent skills for Codex, Claude, and other AI coding agents.

## License

This project is licensed under the [Mozilla Public License 2.0 (MPL 2.0)](LICENSE).

## Skills

| Skill | Description |
|---|---|
| `accessibility-audit` | Audit and fix accessibility issues following WCAG 2.2 Level AA |
| `business-design` | Design business models, validate venture ideas, create structured business plans |
| `context-engineering` | Understand context components, mechanics, and constraints in agent systems |
| `legal-audit` | Audit website legal compliance for Germany — DSGVO, BDSG, DDG, TDDDG, BGB/EGBGB, PAngV, VSBG |
| `legal-pages` | Generate German legal page drafts (Datenschutz, Impressum, AGB, Widerruf, Cookie-Hinweis) |
| `prompt-engineering` | Write and optimize prompts for commands, hooks, skills, and sub-agents |
| `ui-audit` | Audit visual UI quality — layout, spacing, typography, color, consistency |
| `ux-audit` | Audit UX against UX laws, cognitive principles, and Nielsen's heuristics |

## Install

```bash
# All skills
npx skills add idimsh/tdds-business-skills -y

# Single skill
npx skills add idimsh/tdds-business-skills -y --skill accessibility-audit
```

## Structure

Each skill is a directory with a `SKILL.md` file (and optional supporting files):

```
skill-name/
  SKILL.md          # Skill definition (frontmatter + instructions)
  *.md              # Optional support files (e.g. templates)
```
