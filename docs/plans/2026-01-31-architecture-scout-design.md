# Architecture Scout — Plugin Design

A Claude Code plugin that maps, analyzes, and plans migrations for legacy codebases. Distributed through a plugin marketplace.

## Overview

Architecture Scout is a plugin composed of 21 specialized subagents that collaboratively analyze a legacy codebase. Agents communicate through shared markdown files (blackboard pattern). The plugin runs in two phases: a structured scan with parallel/sequential agent waves, followed by an interactive collaborative loop where the user directs deeper investigation.

The user invokes the plugin with `/architecture-scout:scout`. The plugin detects the tech stack, runs a multi-wave analysis, and produces a directory of findings at `docs/architecture-scout/`. After the structured scan, an orchestrator agent enters a collaborative loop with the user to explore findings in depth.

## Marketplace Structure

```
architecture-scout-marketplace/
├── .claude-plugin/
│   └── marketplace.json
└── plugins/
    └── architecture-scout/
        ├── .claude-plugin/
        │   └── plugin.json
        ├── CLAUDE.md
        ├── skills/
        │   └── scout/
        │       ├── SKILL.md
        │       └── stack-profiles/
        │           ├── javascript-typescript.md
        │           ├── python.md
        │           ├── java.md
        │           ├── go.md
        │           └── ruby.md
        └── agents/
            ├── orchestrator.md
            ├── structural/
            │   ├── dependency-mapper.md
            │   ├── boundary-analyzer.md
            │   ├── entry-point-tracer.md
            │   ├── data-flow-tracker.md
            │   ├── config-archaeologist.md
            │   ├── api-surface-mapper.md
            │   └── state-side-effect-mapper.md
            ├── patterns/
            │   ├── pattern-detector.md
            │   ├── inconsistency-finder.md
            │   ├── convention-inferrer.md
            │   └── error-handling-auditor.md
            ├── historical/
            │   └── codebase-historian.md
            ├── health/
            │   ├── tech-debt-assessor.md
            │   ├── testing-surveyor.md
            │   ├── security-flagger.md
            │   └── structural-perf-detector.md
            └── synthesis/
                ├── migration-advisor.md
                ├── quick-wins-identifier.md
                ├── risk-assessor.md
                └── coalescence-agent.md
```

### marketplace.json

```json
{
  "name": "architecture-scout-marketplace",
  "owner": {
    "name": "Ty Wen"
  },
  "metadata": {
    "description": "Tools for analyzing and reviving legacy codebases"
  },
  "plugins": [
    {
      "name": "architecture-scout",
      "source": "./plugins/architecture-scout",
      "description": "Map, analyze, and plan migrations for legacy codebases"
    }
  ]
}
```

### plugin.json

```json
{
  "name": "architecture-scout",
  "description": "Map, analyze, and plan migrations for legacy codebases",
  "version": "0.1.0",
  "author": {
    "name": "Ty Wen"
  },
  "keywords": ["legacy", "architecture", "migration", "analysis", "refactoring"],
  "agents": [
    "./agents/orchestrator.md",
    "./agents/structural/dependency-mapper.md",
    "./agents/structural/boundary-analyzer.md",
    "./agents/structural/entry-point-tracer.md",
    "./agents/structural/data-flow-tracker.md",
    "./agents/structural/config-archaeologist.md",
    "./agents/structural/api-surface-mapper.md",
    "./agents/structural/state-side-effect-mapper.md",
    "./agents/patterns/pattern-detector.md",
    "./agents/patterns/inconsistency-finder.md",
    "./agents/patterns/convention-inferrer.md",
    "./agents/patterns/error-handling-auditor.md",
    "./agents/historical/codebase-historian.md",
    "./agents/health/tech-debt-assessor.md",
    "./agents/health/testing-surveyor.md",
    "./agents/health/security-flagger.md",
    "./agents/health/structural-perf-detector.md",
    "./agents/synthesis/migration-advisor.md",
    "./agents/synthesis/quick-wins-identifier.md",
    "./agents/synthesis/risk-assessor.md",
    "./agents/synthesis/coalescence-agent.md"
  ]
}
```

## Execution Flow

### Phase 0: Detection (inline in skill)

1. User invokes `/architecture-scout:scout`
2. Skill detects tech stack via file existence checks (package.json, go.mod, requirements.txt, etc.)
3. Loads matching stack profile(s) as context
4. Creates `docs/architecture-scout/` output directory
5. Writes initial `index.md` with skeleton sections
6. Asks user: "Detected [stack]. Ready to begin the structured scan?"

### Phase 1: Structured Scan (4 waves)

**Wave 1 — Structural Analysis** (7 agents, all parallel)
- Dependency mapper
- Boundary analyzer
- Entry point tracer
- Data flow tracker
- Config archaeologist
- API surface mapper
- State & side effect mapper

Each writes to `docs/architecture-scout/structural/[agent-name].md`. Checkpoint: summary presented to user, confirmation to continue.

**Wave 2 — Patterns + Historical + Health** (9 agents, all parallel)
These agents can read Wave 1 structural findings before starting.
- Pattern detector
- Inconsistency finder
- Convention inferrer
- Error handling auditor
- Codebase historian
- Tech debt assessor
- Testing surveyor
- Security flagger
- Structural performance detector

Each writes to `docs/architecture-scout/[category]/[agent-name].md`. Checkpoint: summary presented to user, confirmation to continue.

**Wave 3 — Synthesis** (3 agents, sequential)
Each reads all prior findings plus previous synthesis agents' output.
1. Migration advisor → `docs/architecture-scout/synthesis/migration-advisor.md`
2. Quick wins identifier → `docs/architecture-scout/synthesis/quick-wins.md`
3. Risk assessor → `docs/architecture-scout/synthesis/risk-assessment.md`

Checkpoint: summary presented to user, confirmation to continue.

**Wave 4 — Coalescence** (1 agent)
Reads all files, produces the final comprehensive markdown report.
→ `docs/architecture-scout/final-report.md`
→ Updates `docs/architecture-scout/index.md` with links to everything.

### Phase 2: Interactive Loop (orchestrator)

1. Orchestrator reads all findings
2. Proposes 2-3 investigation threads with rationale
3. User picks one or suggests their own
4. Orchestrator dives deep, writes to `docs/architecture-scout/deep-dives/[topic].md`
5. Reports back, proposes next threads
6. Continues until user says stop
7. Updates `docs/architecture-scout/deep-dives/index.md` with links

## Agent Communication Protocol

### Blackboard Pattern

Agents communicate through shared markdown files in `docs/architecture-scout/`.

**Ownership rules**:
- Each agent owns exactly one file
- Only the owning agent writes to its file
- All agents can read any file in the directory

### Agent Output File Format

Every agent output file follows this structure:

```markdown
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
```

### Cross-Referencing

Agents reference other findings by linking to the file and finding title:
`See [Circular dependency in billing](./structural/boundary-analyzer.md#circular-dependency-in-billing)`

### CLAUDE.md

The plugin's `CLAUDE.md` establishes these conventions so every agent inherits them:
- Output directory path convention
- File format template
- Read/write ownership rules
- Cross-referencing syntax
- Stack profile context

## Agent Inventory

### Structural Agents (Wave 1, parallel)

| Agent | Purpose | Output File | Model |
|-------|---------|-------------|-------|
| Dependency mapper | Maps internal/external dependency graph | structural/dependency-mapper.md | haiku |
| Boundary analyzer | Module boundaries, layer violations, circular deps | structural/boundary-analyzer.md | haiku |
| Entry point tracer | Maps all entry points and execution flows | structural/entry-point-tracer.md | haiku |
| Data flow tracker | Follows data transformation through the system | structural/data-flow-tracker.md | haiku |
| Config archaeologist | Maps all configuration surfaces | structural/config-archaeologist.md | haiku |
| API surface mapper | Maps all public APIs and contracts | structural/api-surface-mapper.md | haiku |
| State & side effect mapper | Maps state topology and hidden side effects | structural/state-side-effect-mapper.md | haiku |

### Pattern & Style Agents (Wave 2, parallel)

| Agent | Purpose | Output File | Model |
|-------|---------|-------------|-------|
| Pattern detector | SOLID, DRY, separation of concerns | patterns/pattern-detector.md | sonnet |
| Inconsistency finder | Style drift, naming divergence | patterns/inconsistency-finder.md | sonnet |
| Convention inferrer | Reverse-engineers intended conventions | patterns/convention-inferrer.md | sonnet |
| Error handling auditor | Error handling consistency and quality | patterns/error-handling-auditor.md | sonnet |

### Historical Agent (Wave 2, parallel)

| Agent | Purpose | Output File | Model |
|-------|---------|-------------|-------|
| Codebase historian | Era mapping, abandoned initiatives, knowledge silos | historical/codebase-historian.md | haiku |

### Health Agents (Wave 2, parallel)

| Agent | Purpose | Output File | Model |
|-------|---------|-------------|-------|
| Tech debt assessor | TODOs, dead code, complexity hotspots | health/tech-debt-assessor.md | haiku |
| Testing surveyor | Test coverage patterns and quality | health/testing-surveyor.md | haiku |
| Security flagger | Obvious security surface concerns (observe only) | health/security-flagger.md | sonnet |
| Structural perf detector | Systemic performance patterns only | health/structural-perf-detector.md | sonnet |

### Synthesis Agents (Wave 3, sequential)

| Agent | Purpose | Output File | Model |
|-------|---------|-------------|-------|
| Migration advisor | Non-breaking migration paths | synthesis/migration-advisor.md | inherit |
| Quick wins identifier | Low-effort high-impact improvements | synthesis/quick-wins.md | inherit |
| Risk assessor | Highest-risk areas cross-referenced | synthesis/risk-assessment.md | inherit |

### Final & Interactive Agents

| Agent | Purpose | Output File | Model |
|-------|---------|-------------|-------|
| Coalescence agent | Produces final comprehensive report | final-report.md | inherit |
| Orchestrator | Collaborative loop with user | deep-dives/*.md | inherit |

## Stack Detection & Profiles

### Detection

The skill detects the tech stack by checking for ecosystem marker files:
- `package.json` / `yarn.lock` / `pnpm-lock.yaml` → JavaScript/TypeScript
- `requirements.txt` / `pyproject.toml` / `setup.py` → Python
- `go.mod` → Go
- `pom.xml` / `build.gradle` → Java
- `Gemfile` → Ruby

Multi-stack codebases load all matching profiles.

### Profile Contents (under 200 lines each)

Each profile contains:
- Import patterns to look for
- Package manager signals
- Framework conventions and their legacy indicators
- Configuration surfaces to check
- Testing conventions
- Common migration patterns in that ecosystem

Profiles guide what agents look for, not how they analyze. They are checklists, not tutorials.

## Orchestrator Behavior

The orchestrator has four modes:

1. **Propose threads** — Reads all findings, presents 2-3 investigation options with rationale. Always includes "Or tell me what you'd like to explore."

2. **Deep dive** — Targeted analysis on a user-selected topic. Reads specific files, traces flows, writes detailed findings to `docs/architecture-scout/deep-dives/[topic].md`.

3. **Ask targeted questions** — When hitting ambiguity, asks the user directly. Assumes moderate codebase knowledge. Questions are specific and answerable, not open-ended.

4. **Test hypotheses** — When the orchestrator or user has a theory about the codebase, gathers evidence for/against and reports findings.

**Loop termination**: After each deep dive, asks "Should I keep exploring, or is this a good stopping point?" User can say stop at any time.

## Output Structure

```
docs/architecture-scout/
├── index.md                          # Links to everything
├── final-report.md                   # Comprehensive coalescence report
├── structural/
│   ├── dependency-mapper.md
│   ├── boundary-analyzer.md
│   ├── entry-point-tracer.md
│   ├── data-flow-tracker.md
│   ├── config-archaeologist.md
│   ├── api-surface-mapper.md
│   └── state-side-effect-mapper.md
├── patterns/
│   ├── pattern-detector.md
│   ├── inconsistency-finder.md
│   ├── convention-inferrer.md
│   └── error-handling-auditor.md
├── historical/
│   └── codebase-historian.md
├── health/
│   ├── tech-debt-assessor.md
│   ├── testing-surveyor.md
│   ├── security-flagger.md
│   └── structural-perf-detector.md
├── synthesis/
│   ├── migration-advisor.md
│   ├── quick-wins.md
│   └── risk-assessment.md
└── deep-dives/
    ├── index.md
    └── [topic].md  (created during interactive loop)
```

## Testing Plan

1. Create the marketplace and plugin directory structure
2. Write `marketplace.json` and `plugin.json`
3. Write `CLAUDE.md` with agent conventions
4. Write the scout skill (`SKILL.md`) with stack detection
5. Write stack profiles (start with JavaScript/TypeScript)
6. Write one agent per category as proof of concept
7. Test locally with `claude --plugin-dir ./plugins/architecture-scout`
8. Iterate on agent prompts based on output quality
9. Add remaining agents
10. Test the full flow end-to-end on a real legacy codebase
