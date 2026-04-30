---
name: context-engineering
description: Design, audit, and optimize how AI agents receive and manage context. Use when writing, editing, or optimizing commands, skills, sub-agent prompts, project instruction files, or retrieval pipelines.
---

# Context Engineering

Use this skill to design, audit, or improve how an AI agent receives and manages context: system instructions, tool definitions, retrieved documents, message history, tool outputs, project instruction files, skills, commands, and sub-agent handoffs.

## Agent Portability

Apply these patterns across Codex, Claude, and other agent runtimes. Treat runtime-specific files, commands, tools, and context limits as variables to discover from the environment, not as assumptions.

## Core Principles

- Context is a finite resource with diminishing returns. Every token depletes the attention budget.
- Prefer the smallest high-signal token set that achieves the task.
- Use progressive disclosure: load details only when the current task needs them.
- Place critical instructions at attention-favored positions: beginning and end.
- Keep irrelevant, stale, or conflicting material out of active context.
- Design skills so metadata triggers accurately, then detailed content loads just in time.

## Workflow

1. Identify the context artifact: skill, command, system prompt, sub-agent prompt, project instruction file, retrieval corpus, or handoff.
2. Diagnose the failure mode: too much context, missing context, stale context, conflicting context, poor trigger metadata, weak validation, fragile handoff.
3. Find the matching pattern in this file (degradation, optimization, multi-agent).
4. Produce a concise diagnosis with concrete changes and a reason for each.
5. Prefer edits that reduce active context while improving routing, validation, and just-in-time retrieval.
6. If validating, use independent runs or sub-agents only when the runtime supports them and the validation does not leak the expected answer.

---

## Context Anatomy

| Component | Role | Budget Pressure |
|-----------|------|-----------------|
| System prompt | Core identity, constraints, behavior | Low token count, high attention |
| Tool definitions | Available actions + descriptions | Front-loaded; poor descriptions force guessing |
| Retrieved documents | Domain knowledge loaded at runtime | High if unfiltered; use just-in-time loading |
| Message history | Conversation + scratchpad memory | Grows linearly; dominates in long sessions |
| Tool outputs | Results of agent actions | Can reach 80%+ of total tokens |

### System Prompt Structure

Organize with clear section boundaries. The exact format (XML tags, Markdown headers) matters less than structural clarity.

**Right altitude:** Specific enough to guide behavior, flexible enough to provide strong heuristics. Avoid both brittle hardcoded logic and vague high-level hand-waving.

### Tool Descriptions

Tool descriptions steer agent behavior. If a human cannot tell which tool to use in a given situation, an agent will not do better. Include usage context, examples, and defaults.

```markdown
# Good
description: Analyze code architecture. Use for design reviews.

# Bad
description: This skill provides comprehensive analysis of code
architecture including but not limited to class hierarchies,
dependency graphs, coupling metrics, cohesion analysis...
```

### Progressive Disclosure in Practice

- At startup: load only skill names and descriptions.
- On activation: load full skill content.
- For reference data: maintain lightweight identifiers (paths, queries, links) and load on demand.

```markdown
# Step 1: Load summary
docs/architecture_overview.md

# Step 2: Load detail only when needed
docs/api/endpoints.md        # Only for API work
docs/database/schemas.md     # Only for data layer work
```

### Hybrid Loading Strategy

Pre-load stable context for speed (project instruction files like CLAUDE.md, AGENTS.md). Enable autonomous exploration for dynamic or highly specific information. Decision boundary: stable content pre-loads; changing content loads just-in-time.

### Context Budgeting

Know the effective limit for your model and task. Monitor usage during development. Implement compaction triggers at 70-80% utilization. Design assuming context will degrade.

---

## Attention Budget and Positioning

Models show U-shaped attention: beginning and end get reliable attention; the middle suffers 10-40% lower recall. The first token acts as an "attention sink" that absorbs disproportionate budget.

**Placement rules:**
- Critical constraints and instructions go at the START.
- Key reminders and output format go at the END.
- Detailed guidelines, examples, and reference material go in the MIDDLE.
- For long documents, surface key information at edges via summaries.

```markdown
<CRITICAL_CONSTRAINTS>                    # Start (high attention)
- Never modify production files directly
- Always run tests before committing
</CRITICAL_CONSTRAINTS>

<DETAILED_GUIDELINES>                     # Middle (lower attention)
- Code style, templates, checklists
</DETAILED_GUIDELINES>

<KEY_REMINDERS>                           # End (high attention)
- Run tests: npm test
- Create PR with description
</KEY_REMINDERS>
```

---

## Degradation Patterns

Context degrades predictably. Recognize these patterns to diagnose and fix failures.

### Lost-in-Middle

Information in the center of context receives 10-40% lower recall than the same information at the beginning or end. This is a consequence of attention mechanics, not a bug.

**Mitigation:**
- Place critical instructions at beginning and end of context.
- Use explicit section headers to help models navigate.
- For long documents, surface key information in summaries at attention-favored positions.
- Add a verification checklist at the end that references critical middle-section items.
- Add emphasis markers: `[REQUIRED]`, `[CRITICAL]`, bold text.

### Context Poisoning

Hallucinations or errors enter context and compound through repeated reference. Enters via bad tool outputs, incorrect retrieved docs, or hallucinated intermediate results.

**Symptoms:** Degraded output on previously-successful tasks, persistent hallucinations despite correction, wrong tool calls.

**Recovery:**
- Truncate context to before the poisoning point.
- Explicitly flag the error and request re-evaluation.
- Restart with clean context, preserving only verified information.

### Context Distraction

Irrelevant information competes for attention budget. Even a single irrelevant document reduces performance on relevant tasks. Models cannot "skip" irrelevant context -- they attend to everything provided.

**Mitigation:**
- Filter for relevance before loading retrieved documents.
- Use namespacing and clear organization.
- Move information to on-demand tool calls instead of pre-loading.

### Context Confusion

The model cannot determine which context applies to the current situation. Responses address the wrong aspect, use inappropriate tools, or mix requirements from multiple sources.

**Mitigation:**
- Explicit task segmentation -- different tasks get different context windows.
- Clear transitions between task contexts.
- State management that isolates context for different objectives.

### Context Clash

Multiple correct pieces of information contradict each other (version conflicts, multi-source retrieval, perspective conflicts).

**Mitigation:**
- Explicit conflict marking -- identify contradictions and request clarification.
- Priority rules -- establish which source takes precedence.
- Version filtering -- exclude outdated information.

### Degradation Warning Signs

| Utilization | Symptoms |
|-------------|----------|
| 50-70% | Occasional missed instructions, less focused responses |
| 70-85% | Inconsistent behavior, "forgotten" earlier instructions |
| 85%+ | Key constraints ignored, hallucinations increase, task failure |

---

## Optimization Techniques

### The Four-Bucket Framework

| Strategy | When to Use | What It Does |
|----------|-------------|--------------|
| **Write** | Preserve info without consuming context | Save to scratchpad, file system, external storage |
| **Select** | Context contains irrelevant material | Pull only relevant context via retrieval and filtering |
| **Compress** | Context too verbose but information needed | Summarize, abstract, mask observations |
| **Isolate** | Single context growing too large | Split across sub-agents or sessions |

### Compaction

Summarize context contents when approaching limits, then reinitialize with the summary. Priority for compression:

1. **Tool outputs** -- replace verbose outputs with key findings
2. **Old conversation turns** -- summarize early exchanges
3. **Retrieved documents** -- summarize if task context captured
4. **Never compress** -- system prompt and critical constraints

Trigger compaction at 70-80% utilization, before degradation becomes severe. Effective summaries preserve: key findings and metrics from tool outputs, decisions and commitments from conversations, key facts from documents.

### Observation Masking

Tool outputs can reach 80%+ of tokens. Replace verbose outputs with compact references once they have served their purpose.

- **Never mask:** Observations critical to current task, most recent turn, active reasoning
- **Mask after use:** Outputs from 3+ turns ago, verbose outputs with key points already extracted, outputs already summarized
- **Always mask:** Repeated outputs, boilerplate headers/footers

### Context Partitioning (Sub-Agents)

Split work across sub-agents with isolated contexts. Each operates in a clean context focused on its subtask.

**When to partition:**
- Task naturally decomposes into independent subtasks
- Different subtasks need different specialized context
- Context accumulation threatens limits
- Subtasks have conflicting requirements

**Example -- sub-agent isolation:**
```markdown
## Coordinator Agent (lean context)
- Task decomposition
- Delegates to specialized sub-agents
- Synthesizes results

## Code Review Sub-Agent (isolated context)
- Only code review guidelines loaded
- Returns structured findings

## Test Writer Sub-Agent (isolated context)
- Only testing patterns loaded
- Returns test files
```

**Result aggregation:** Validate all partitions completed, merge compatible results, summarize if combined results too large, resolve conflicts.

### Optimization Decision Framework

| Dominant Component | Apply |
|-------------------|-------|
| Tool outputs | Observation masking |
| Retrieved documents | Summarization or partitioning |
| Message history | Compaction with summarization |
| Multiple components | Combine strategies |

### Prompt Optimization

**Commands:** Keep individual commands focused on a single concern.

```markdown
# Good: Focused
name: review-security
description: Review code for security vulnerabilities

# Bad: Overloaded
name: review-all
description: Review code for everything
```

**Sub-agent handoffs:** Provide focused, minimal context.

```markdown
# Good
"Review authentication module for security issues. Return findings in structured format."

# Bad
"I need you to look at the authentication module which is located in src/auth/ and contains
several files including login.ts, session.ts, tokens.ts... [500 more tokens]"
```

---

## Verification Workflows

Use these patterns when agent output feeds downstream agents, when validating long prompts, or during post-mortem analysis. Skip if the runtime does not support sub-agents -- use manual review instead.

### Hallucination Detection

1. Have the primary agent complete its task.
2. Extract factual claims (file paths, function names, code behavior assertions, external facts, metrics).
3. Verify each claim category: file paths via file tools, code claims via reading actual code, external facts via documentation.
4. Calculate poisoning risk: `(false_claims * 2 + unverifiable_claims) / total_claims`
5. Risk < 0.1: proceed. Risk 0.1-0.3: review flagged claims. Risk > 0.3: regenerate with explicit grounding instructions listing the specific false claims found.

### Lost-in-Middle Testing

Test whether a prompt's critical instructions are reliably followed:

1. Extract all critical instructions from the prompt.
2. Run 3-5 agents with the SAME prompt and identical inputs.
3. For each run, verify compliance with every critical instruction.
4. Calculate compliance rate per instruction: `followed / applicable_runs`
5. Classify: 100% = reliable, 80%+ = mostly reliable, 50-79% = at-risk (lost-in-middle), <50% = frequently ignored.

**Remediation for at-risk instructions:**
- Move to beginning or end of prompt.
- Add emphasis markers: `[REQUIRED]`, `[CRITICAL]`, bold text.
- Split prompt into focused sub-prompts.
- Add explicit "verify these items" reminder at end.

### Error Propagation Tracing

For multi-agent chains where final output has errors:

1. Record output of each agent in the chain.
2. Identify errors in the final output.
3. Trace each error backward: present in output but not input = agent INTRODUCED it; present in both = agent PROPAGATED it.
4. Add verification checkpoints after agents that frequently introduce errors.
5. Only proceed to the next agent if verification passes.

### Context Health Check

For long-running sessions (20+ turns), check every ~10 turns:

- **Lost-in-middle:** Agent missing instructions from early in conversation, asking for already-provided information.
- **Poisoning:** Same error repeating, hallucinations persisting despite correction.
- **Distraction:** Responses becoming unfocused, irrelevant context used inappropriately.
- **Confusion:** Mixing up different task requirements, wrong tool selections.
- **Clash:** Uncertainty about conflicting information, inconsistent behavior.

**Intervention:** If degraded or critical, extract essential state to a file, start a new session with clean context, and load preserved state.

---

## Skill Design Checklist

- [ ] Frontmatter `description` clearly says what the skill does and when to use it.
- [ ] Body stays lean -- only actionable instructions, not background theory.
- [ ] Instructions are agent-neutral unless a runtime-specific variant is required.
- [ ] Examples are concise and representative, not exhaustive.
- [ ] Critical instructions appear at beginning and end, not buried in the middle.
- [ ] Validation steps catch hallucinations, regressions, and context-size failures.
- [ ] Progressive disclosure: detailed content loads only when needed.
- [ ] Context budget considered: every section justifies its token cost.

## Multi-Agent Verification Guidelines

1. Spawn verification agents with focused, single-purpose prompts.
2. Use structured output formats for reliable parsing.
3. Set clear thresholds for action vs. continue decisions.
4. Balance verification overhead against error prevention value.
5. Implement verification at natural checkpoints, not every turn.
6. Use lighter checks for routine operations, heavier for critical ones.

## Key Reminders

- Context quality matters more than quantity.
- Design for degradation -- assume it will happen, not hope it will not.
- Measure before optimizing: know your current context utilization.
- Every token in context must earn its place.
