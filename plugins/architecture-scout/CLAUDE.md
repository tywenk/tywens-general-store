# Architecture Scout — Agent Conventions

This plugin uses a blackboard communication pattern. All agents write findings to markdown files in the target project's `docs/architecture-scout/` directory.

## Output Directory

All agent output goes to `docs/architecture-scout/` in the target project root. Subdirectories mirror the agent categories:

- `structural/` — Wave 1 structural analysis agents
- `patterns/` — Wave 2 pattern and style agents
- `historical/` — Wave 2 codebase history agent
- `health/` — Wave 2 health and debt agents
- `synthesis/` — Wave 3 synthesis agents
- `deep-dives/` — Interactive loop deep dive documents

## File Ownership

- Each agent owns exactly one output file
- Only the owning agent writes to its file
- All agents may read any file in `docs/architecture-scout/`

## Output File Format

Every agent output file MUST follow this structure:

# [Agent Name] Findings

## Summary
Brief 2-3 sentence overview of key findings.

## Confidence
High / Medium / Low — how thorough was the analysis.

## Findings

### [Finding Title]
- **Severity**: Critical / Warning / Info
- **Location**: file paths and line references
- **Description**: what was found
- **Evidence**: code snippets or patterns observed
- **Recommendation**: suggested action (if applicable)

## Cross-References
Links to related findings in other agent files.

## Open Questions
Things this agent couldn't determine that the user or other agents might clarify.

## Cross-Referencing

When referencing findings from other agents, use relative markdown links:
`See [Finding Title](./category/agent-file.md#finding-title-slug)`

## Wave Dependencies

- Wave 1 (structural) agents: no dependencies, run in parallel
- Wave 2 (patterns, historical, health) agents: should read Wave 1 output before analysis
- Wave 3 (synthesis) agents: should read all Wave 1 and Wave 2 output, run sequentially
- Wave 4 (coalescence): reads all files, produces final report
- Orchestrator: reads all files, interactive with user
