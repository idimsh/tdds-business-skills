---
name: prompt-engineering
description: Use this skill when writing commands, hooks, skills for AI agents, sub-agent prompts, or any other LLM interaction, including optimizing prompts, improving LLM outputs, or designing production prompt templates.
---

# Prompt Engineering

Use this skill to design, review, or optimize prompts for AI agents, sub-agents, command files, hooks, production prompt templates, RAG workflows, structured outputs, and validation loops.

## Agent Portability

Apply these patterns across Codex, Claude, and other agent runtimes. Treat model names, context limits, tool syntax, slash commands, hooks, and project instruction files as runtime-specific details to discover before writing final prompts.

## Core Principles

- Be specific about task, inputs, constraints, tools, and output format.
- Use examples when consistency matters; keep them representative and few.
- Set the right degree of freedom: high for judgment tasks, low for fragile operations.
- Keep prompts concise. Do not explain what the target model already knows.
- Make recovery behavior explicit: missing data, uncertainty, validation failures, fallback paths.
- Treat prompts as versioned engineering artifacts when used in production.

## Workflow

1. Identify the prompt type: system prompt, command, skill, hook, sub-agent prompt, production template, or one-off task prompt.
2. Clarify the target runtime and constraints: available tools, context limit, safety constraints, project instruction files, expected output channel.
3. Draft or revise the prompt with clear sections: role/context, task, inputs, constraints, process, output format, validation.
4. Remove low-signal background and redundant explanations.
5. Add test cases or evaluation criteria for production prompts.

---

## Prompt Structure

### Instruction Hierarchy

```
[System Context] -> [Task Instruction] -> [Examples] -> [Input Data] -> [Output Format]
```

### System Prompt Design

Set global behavior and constraints that persist across the conversation. Define role, expertise level, output format, and safety guidelines. Use system prompts for stable instructions; free up user message tokens for variable content.

```markdown
System: You are a senior backend engineer specializing in API design.

Rules:
- Always consider scalability and performance
- Suggest RESTful patterns by default
- Flag security concerns immediately
- Provide code examples in Python

Format responses as:
1. Analysis
2. Recommendation
3. Code example
4. Trade-offs
```

### Section Organization

Use XML tags or Markdown headers to delineate sections. Structural clarity aids attention:

```markdown
<BACKGROUND_INFORMATION>
You are a Python expert. Project: data pipeline in Python 3.9+
</BACKGROUND_INFORMATION>

<INSTRUCTIONS>
- Write clean, idiomatic Python with type hints
- Add docstrings for public functions
- Follow PEP 8
</INSTRUCTIONS>

<OUTPUT_DESCRIPTION>
Provide actionable feedback with specific line references.
</OUTPUT_DESCRIPTION>
```

---

## Conciseness

The context window is shared with system prompts, conversation history, other skills, tool outputs, and the agent's own reasoning. Every token in your prompt competes with everything else.

**Default assumption:** The target model is already capable. Only add context it does not already have.

Challenge each piece of information:
- Does the model need this explanation?
- Can I assume it knows this?
- Does this paragraph justify its token cost?

````markdown
# Good (~50 tokens)
## Extract PDF text
Use pdfplumber for text extraction:

```python
import pdfplumber

with pdfplumber.open("file.pdf") as pdf:
    text = pdf.pages[0].extract_text()
```

# Bad (~150 tokens)
## Extract PDF text
PDF (Portable Document Format) files are a common file format...
There are many libraries available... we recommend pdfplumber
because... First, you'll need to install it using pip...
````

---

## Degree of Freedom Calibration

Match specificity to the task's fragility and variability.

**High freedom** -- multiple valid approaches, decisions depend on context:

```markdown
## Code review process
1. Analyze code structure and organization
2. Check for potential bugs or edge cases
3. Suggest improvements for readability
4. Verify adherence to project conventions
```

**Medium freedom** -- preferred pattern exists, some variation acceptable:

````markdown
## Generate report
Use this template, customize as needed:

```python
def generate_report(data, format="markdown", include_charts=True):
    # Process data, generate output, optionally include visualizations
```
````

**Low freedom** -- fragile operations, consistency critical, specific sequence required:

````markdown
## Database migration
Run exactly this script:

```bash
python scripts/migrate.py --verify --backup
```

Do not modify the command or add additional flags.
````

**Mental model:** Narrow bridge with cliffs (low freedom, exact instructions) vs. open field (high freedom, general direction).

---

## Key Techniques

### Few-Shot Learning

Show 2-5 input-output examples instead of explaining rules. More examples improve accuracy but consume tokens -- balance based on task complexity.

```markdown
Extract key information from support tickets:

Input: "My login doesn't work and I keep getting error 403"
Output: {"issue": "authentication", "error_code": "403", "priority": "high"}

Input: "Feature request: add dark mode to settings"
Output: {"issue": "feature_request", "error_code": null, "priority": "low"}

Now process: "Can't upload files larger than 10MB, getting timeout"
```

### Chain-of-Thought

Request step-by-step reasoning before the final answer. Use for complex problems requiring multi-step logic. Improves accuracy on analytical tasks by 30-50%.

```markdown
Analyze this bug report and determine root cause.

Think step by step:
1. What is the expected behavior?
2. What is the actual behavior?
3. What changed recently that could cause this?
4. What components are involved?
5. What is the most likely root cause?

Bug: "Users can't save drafts after the cache update deployed yesterday"
```

### Progressive Disclosure

Start simple, add complexity only when needed:

1. **Level 1:** Direct instruction -- "Summarize this article"
2. **Level 2:** Add constraints -- "Summarize in 3 bullet points, focusing on key findings"
3. **Level 3:** Add reasoning -- "Identify the main findings, then summarize each in one bullet"
4. **Level 4:** Add examples -- include 2-3 input-output pairs

### Error Recovery

Build prompts that handle failures gracefully:

- Include fallback instructions for missing data.
- Request confidence scores when uncertainty matters.
- Ask for alternative interpretations when ambiguous.
- Specify how to indicate missing information (say "I cannot verify..." instead of asserting).

### Template Systems

Build reusable prompt structures with variables for repeated patterns:

```python
template = """
Review this {language} code for {focus_area}.
Code: {code_block}
Provide feedback on: {checklist}
"""

prompt = template.format(
    language="Python",
    focus_area="security vulnerabilities",
    code_block=user_code,
    checklist="1. SQL injection\n2. XSS risks\n3. Authentication"
)
```

---

## Persuasion Principles for Prompt Design

LLMs respond to the same persuasion patterns as humans. Research shows these techniques more than doubled compliance rates (33% to 72%). Use them to ensure critical practices are followed, not to manipulate.

### Authority

Use imperative language for discipline-enforcing instructions. "YOU MUST", "Never", "Always", "No exceptions" -- eliminates decision fatigue and rationalization.

```markdown
# Strong compliance
Write code before test? Delete it. Start over. No exceptions.

# Weak compliance
Consider writing tests first when feasible.
```

### Commitment

Require explicit announcements and choices. Forces consistency with stated intentions.

```markdown
# Strong
When you find a skill, you MUST announce: "I'm using [Skill Name]"

# Weak
Consider letting your partner know which skill you're using.
```

### Scarcity (Urgency)

Time-bound requirements prevent "I'll do it later" drift.

```markdown
# Strong
After completing a task, IMMEDIATELY request code review before proceeding.

# Weak
You can review code when convenient.
```

### Social Proof

Establish norms by stating universal patterns and failure modes.

```markdown
# Strong
Checklists without tracking = steps get skipped. Every time.

# Weak
Some people find tracking helpful for checklists.
```

### Unity

Collaborative language for non-hierarchical practices.

```markdown
# Good
We're colleagues working together. I need your honest technical judgment.
```

### Principles to Avoid

- **Reciprocity:** Rarely effective in prompts; skip it.
- **Liking:** Creates sycophancy, conflicts with honest feedback. Never use for discipline enforcement.

### Principle Selection by Prompt Type

| Prompt Type | Use | Avoid |
|------------|-----|-------|
| Discipline-enforcing | Authority + Commitment + Social Proof | Liking, Reciprocity |
| Guidance/technique | Moderate Authority + Unity | Heavy authority |
| Collaborative | Unity + Commitment | Authority, Liking |
| Reference docs | Clarity only | All persuasion |

### Why These Work

- Bright-line rules ("YOU MUST") remove rationalization and decision fatigue.
- Implementation intentions ("When X, do Y") create automatic execution, more effective than "generally do Y".
- Authority language precedes compliance in LLM training data; commitment sequences and social proof patterns establish norms.

### Implementation Pattern

Combine: clear trigger + required action + no exceptions.

**Ethics test:** Would this technique serve the user's genuine interests if they fully understood it? Legitimate: ensuring critical practices, preventing predictable failures. Illegitimate: false urgency, guilt-based compliance.

---

## Integration Patterns

### With RAG Systems

```python
prompt = f"""Given the following context:
{retrieved_context}

{few_shot_examples}

Question: {user_question}

Answer based solely on the context above. If the context doesn't contain
enough information, explicitly state what's missing."""
```

### With Self-Verification

```python
prompt = f"""{main_task_prompt}

After generating your response, verify:
1. Answers the question directly
2. Uses only information from provided context
3. Cites specific sources
4. Acknowledges any uncertainty

If verification fails, revise your response."""
```

---

## Token Efficiency

- Remove redundant words and phrases.
- Use abbreviations consistently after first definition.
- Consolidate similar instructions.
- Move stable content to system prompts.
- Cache common prompt prefixes.
- Batch similar requests when possible.

---

## Prompt Optimization Process

1. Start simple, measure performance (accuracy, consistency, token usage).
2. Test on diverse inputs including edge cases.
3. Iterate: small changes can have large impact.
4. A/B test variations for production prompts.
5. Version control prompts as code.

---

## Common Pitfalls

- **Over-engineering:** Starting with complex prompts before trying simple ones.
- **Example pollution:** Examples that do not match the target task.
- **Context overflow:** Exceeding token limits with excessive examples or background.
- **Ambiguous instructions:** Leaving room for multiple interpretations on fragile operations.
- **Ignoring edge cases:** Not testing on unusual or boundary inputs.
- **Hidden assumptions:** Assuming context the model does not have.
- **Weak compliance language:** Using "consider" or "when feasible" for mandatory requirements.

---

## Review Checklist

- [ ] Trigger or task description is concrete enough for an agent to choose this prompt correctly.
- [ ] Required inputs and unavailable-input behavior are specified.
- [ ] Tool-use guidance is explicit but not runtime-locked unless necessary.
- [ ] Output format is unambiguous.
- [ ] Validation is included for high-risk or repeatable workflows.
- [ ] No unnecessary verbosity, hidden assumptions, or conflicting instructions.
- [ ] Degree of freedom matches task fragility.
- [ ] Critical instructions are at beginning and end, not buried in the middle.
- [ ] Examples are representative and few, not exhaustive.
- [ ] Recovery behavior is explicit for missing data and edge cases.
- [ ] Persuasion principles match prompt type (authority for discipline, unity for collaboration).
- [ ] Production prompts have test cases and evaluation criteria.
