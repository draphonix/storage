# Documentation Target Mapping

## By Topic Type

| Topic Type | Target Files |
|------------|--------------|
| Architecture | `AGENTS.md`, `docs/ARCHITECTURE.md` |
| CLI commands | `packages/cli/AGENTS.md` |
| SDK/API | `packages/sdk/AGENTS.md` |
| Patterns | `.claude/skills/*/SKILL.md` |
| Quick start | `README.md` |
| Module dev | `packages/sdk/docs/MODULE_DEVELOPMENT.md` |
| Troubleshooting | `docs/TROUBLESHOOTING.md` |
| Deployment | `docs/DEPLOYMENT.md` |
| Migration | `docs/MIGRATION.md` |

## By Change Type

| Change | Primary Doc | Secondary |
|--------|-------------|-----------|
| New command | Package AGENTS.md | README.md |
| New hook | SDK AGENTS.md | Skill if pattern |
| Breaking change | MIGRATION.md | Affected AGENTS.md |
| New pattern | Relevant SKILL.md | AGENTS.md |
| Bug fix pattern | TROUBLESHOOTING.md | - |
| Config change | README.md | DEPLOYMENT.md |

## Section Conventions

### AGENTS.md
- Commands table with examples
- Architecture diagrams (mermaid)
- Dependency rules tables
- Quick reference tables

### README.md
- Quick start commands
- Feature lists (brief)
- Installation steps
- Links to detailed docs

### SKILL.md
- When to use section
- Patterns with examples
- Reference tables
- Troubleshooting section
