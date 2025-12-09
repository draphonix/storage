---
description: File detailed Beads epics and issues from a plan
argument-hint: <plan-description-or-context>
---

# File Beads Epics and Issues from Plan

You are tasked with converting a plan into a comprehensive set of Beads epics and issues. Follow these steps carefully:

## Step 1: Understand the Plan

First, review the plan context provided: `$ARGUMENTS`

If no specific plan is provided, ask the user to share the plan or point to a planning document (check `history/` directory for recent plans).

## Step 2: Analyze and Structure

Before filing any issues, analyze the plan for:

1. **Major workstreams** - These become epics
2. **Individual tasks** - These become issues under epics
3. **Dependencies** - What must complete before other work can start?
4. **Parallelization opportunities** - What can be worked on simultaneously?
5. **Technical risks** - What needs spikes or investigation first?

## Step 3: File Epics First

Create epics for major workstreams using:

```bash
bd create "Epic: <title>" -t epic -p <priority> --json
```

Epics should:

- Have clear, descriptive titles
- Include acceptance criteria in the description
- Be scoped to deliverable milestones

## Step 4: File Detailed Issues

For each epic, create child issues with:

```bash
bd create "<task title>" -t <type> -p <priority> --deps <parent-epic-id> --json
```

Each issue MUST include:

- **Clear title** - Action-oriented (e.g., "Implement X", "Add Y", "Configure Z")
- **Detailed description** - What exactly needs to be done
- **Acceptance criteria** - How do we know it's done?
- **Technical notes** - Implementation hints, gotchas, relevant files
- **Dependencies** - Link to blocking issues with `--deps bd-<id>`

## Step 5: Map Dependencies Carefully

For each issue, consider:

- Does this depend on another issue completing first?
- Can this be worked on in parallel with siblings?
- Are there cross-epic dependencies?

Use `--deps bd-X,bd-Y` for multiple dependencies.

## Step 6: Set Priorities Thoughtfully

- `0` - Critical path blockers, security issues
- `1` - Core functionality, high business value
- `2` - Standard work items (default)
- `3` - Nice-to-haves, polish
- `4` - Backlog, future considerations

## Step 7: Verify the Graph

After filing all issues, run:

```bash
bd list --json
bd ready --json
```

Verify:

- All epics have child issues
- Dependencies form a valid DAG (no cycles)
- Ready work exists (some issues have no blockers)
- Priorities make sense for execution order

## Output Format

After completing, provide:

1. Summary of epics created
2. Summary of issues per epic
3. Dependency graph overview (what unblocks what)
4. Suggested starting points (ready issues)
5. Parallelization opportunities
