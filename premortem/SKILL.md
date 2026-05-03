---
name: premortem
description: "Run a premortem on any plan, launch, product, hire, strategy, or decision. Assumes it already failed 6 months from now and works backward to find every reason why. Produces a revised plan with blind spots exposed. MANDATORY TRIGGERS: 'premortem this', 'premortem my', 'run a premortem', 'what could kill this', 'future-proof this', 'stress test this plan', 'what am i missing here', 'find the blind spots'. STRONG TRIGGERS: 'what could go wrong', 'am i missing anything', 'poke holes in this', 'where will this break', 'devil's advocate this'. Do NOT trigger on simple feedback requests, factual questions, or LLM Council requests. DO trigger when someone has a plan or commitment where the cost of being wrong is high."
---

# Premortem

A premortem is the opposite of a postmortem. Instead of figuring out what went wrong after something fails, you imagine it already failed and figure out why before you start.

The method comes from psychologist Gary Klein. He published it in Harvard Business Review. Daniel Kahneman (the Nobel Prize-winning psychologist behind "Thinking, Fast and Slow") called it his single most valuable decision-making technique. Google, Goldman Sachs, and Procter & Gamble all use it before major decisions.

The core insight: when you ask people "what could go wrong?" they give you cautious, hedged answers. When you say "this already failed, tell me why," their brains switch into narrative mode and generate way more specific, creative, honest reasons. Researchers at Wharton and Cornell called this "prospective hindsight" and found it significantly increases the ability to identify causes of future outcomes.

The reason this matters for AI-assisted decisions: Claude defaults to agreeable, optimistic responses. If you ask "is this a good plan?" it will find reasons to say yes. The premortem breaks this pattern by forcing the frame into "this is dead, explain how it died." Claude stops looking for reasons your plan will work and starts explaining how it fell apart.

Run a premortem when the user has a concrete plan or commitment and needs the honest version of "how this fails" before execution. The job is not general brainstorming or light feedback. The job is to assume failure, identify the real reasons it died, and turn that into a more resilient plan.

## Project Context

$ARGUMENTS

## Agent Portability

This skill must work across Codex, Claude, and other SKILL.md-compatible agents.

- Do not assume slash commands or a specific runtime.
- Read project instructions and context files when present: `AGENTS.md`, `CLAUDE.md`, `README.md`, `.cursor/rules`, `.windsurfrules`, planning docs, briefs, and files the user referenced.
- Use the available search and file-read tools for quick context gathering.
- If parallel sub-agents are available, use them for the deep dives. If not, do the deep dives sequentially in the main agent.

## When To Use

Good premortem targets:

- A product or feature you're about to build
- A launch plan with money or reputation on the line
- A pricing change or business model shift
- A hire you're about to make
- A strategy or positioning pivot
- A partnership or deal you're evaluating
- Any commitment where the cost of being wrong is high

Do not use this skill for:

- Vague ideas with no concrete plan yet (help them plan first, then premortem)
- Questions with one right answer (just answer them)
- Requests for creative feedback on a draft (that's editing, not a premortem)
- Decisions that are already made and irreversible (a premortem is only useful when you can still change course)

## Context Gathering (The Minimum Bar)

A premortem is only as good as the context it runs on. Vague input produces vague failure scenarios that help nobody. Before running the premortem, you need to hit a minimum context threshold.

### Step 1: Scan for existing context

Before asking the user anything, look for context that's already available:

**A. The current conversation.** The user may have been discussing a plan, a launch, a product, or a decision earlier in this session. Read back through the conversation and extract whatever's relevant.

**B. The workspace.** Quickly scan for files that might contain relevant context:

- `CLAUDE.md` or `claude.md` (business context, preferences, constraints)
- Any `memory/` folder (audience profiles, business details, past decisions)
- Files the user explicitly referenced or attached
- Any project files, briefs, or plans that relate to the thing being premortemed

Use `Glob` and quick `Read` calls. Don't spend more than 30 seconds on this. You're looking for the key files that would ground the failure scenarios in reality.

### Step 2: Evaluate context sufficiency

After scanning, check whether you have enough to run a useful premortem. You need three things:

1. **What is it?** — A clear understanding of the thing being premortemed (a product, a launch, a hire, a pricing change, a strategy). You need to be able to describe it back to the user in one sentence.
2. **Who is it for / who does it affect?** — The audience, the customer, the team, the stakeholders. Failure scenarios depend heavily on who's involved.
3. **What does success look like?** — What outcome is the user hoping for? Failure is defined by inverting success. If you don't know what success means, you can't define what failure means.

### Step 3: Fill gaps conversationally

If you have all three, proceed immediately to the premortem. Don't ask unnecessary questions.

If you're missing one or more, ask for the most important missing piece first. One question at a time. Evaluate after each answer whether you now have enough. Keep asking until the threshold is met, but never ask more than you need.

Examples of focused context questions:

- "What specifically are you about to launch/build/decide?" (if you don't know what it is)
- "Who is this for?" (if you know the plan but not the audience)
- "What does a win look like for this?" (if you know the plan and audience but not the success criteria)

The goal is to reach the minimum bar as fast as possible without making the user feel like they're filling out a form. Conversational, not interrogative. If you can infer an answer from context, do that instead of asking.

## Workflow

### Step 1: Set The Frame

After gathering sufficient context, set the premortem frame explicitly. Something like:

"OK, I have enough context. Let's run the premortem. Here's the premise: it's 6 months from now. [The plan/launch/decision] has failed. It's done. We're looking back and trying to understand what went wrong."

This framing matters. It shifts the mode from "evaluate this plan" (which triggers agreeable responses) to "explain why this died" (which triggers honest, specific failure identification). Do not drift into generic risk assessment or agreeable feedback.

### Step 2: Generate Raw Failure Reasons

Produce a comprehensive list of genuine failure reasons for this specific plan.

Rules:

- Ground every reason in the actual plan details
- Prefer specific threats over generic advice
- Include every real failure mode you see, but do not pad the list
- State each failure reason in 1-2 sentences

The right number of failure modes depends on the plan. Some plans have 4 real threats. Others have 9.

### Step 3: Deep-Dive Each Failure Reason

Take each failure reason from Step 2 and analyze it independently.

If sub-agents are available, spawn one sub-agent per failure reason in parallel. If not, perform one deep dive per reason in the main agent while keeping them analytically separate.

Use this prompt structure for each deep dive:

```text
You are investigating one failure mode in a premortem.

PLAN CONTEXT
[What it is, who it is for, what success looks like, plus any relevant workspace context]

PREMORTEM FRAME
It is 6 months from now. This plan has failed.

ASSIGNED FAILURE REASON
[One specific failure reason]

Write:
1. The failure story: 2-3 paragraphs describing how this failure actually played out.
2. The underlying assumption: one sentence describing what the user assumed that made this failure possible.
3. Early warning signs: 1-2 concrete observable signals that this failure is starting.

Keep it under 300 words. Be direct, specific, and unsentimental.
```

### Step 4: Synthesize

After all deep dives are done, produce a `PREMORTEM REPORT` with:

1. `The Most Likely Failure` - the scenario most likely to happen first
2. `The Most Dangerous Failure` - the scenario with the highest downside
3. `The Hidden Assumption` - the biggest unexamined assumption across all analyses
4. `The Revised Plan` - specific changes that make the plan more resilient
5. `The Pre-Launch Checklist` - 3-5 checks, tests, or safeguards to put in place before execution

Every recommendation in the revised plan should map directly to one or more identified failure modes.

### Step 5: Save A Visual Report

Generate a self-contained HTML report in the user's workspace:

```text
premortem-report-[timestamp].html
```

Requirements:

- Inline CSS only
- Dark background, high readability, clear hierarchy
- Synthesis section at the top
- One card per failure reason
- Each card includes the failure reason, failure story, underlying assumption, and early warning signs
- Show likelihood and severity clearly for each failure mode
- Include a footer with timestamp and subject

Open the file after generating it when the runtime supports that.

### Step 6: Save The Transcript

Save the full working record as:

```text
premortem-transcript-[timestamp].md
```

Include:

- Context gathered
- Raw failure reasons
- All deep dives
- Full synthesis

## Chat Output

Also give the user a concise chat summary in three sentences or less:

- The most likely failure
- The hidden assumption
- The single highest-leverage revision

The HTML report is for scanning. The transcript is for detail.

## Important Notes

- **Always spawn all failure agents in parallel.** Sequential spawning wastes time and lets earlier responses influence later ones. If sub-agents are not available, keep the deep dives analytically separate so earlier ones do not contaminate later ones.
- **Always set the premortem frame explicitly.** "This has already failed" is the psychological mechanism that makes this work. Without it, the analysis defaults to polite risk assessment instead of honest failure identification.
- **Be comprehensive but not padded.** Find every genuine failure reason. Don't stop at 3 if there are 7. But don't force 7 if there are only 3. The number should be whatever is real for this specific plan.
- **The synthesis is the product.** Most users will read the synthesis and skim the individual failure cards. Make the synthesis specific and actionable.
- **Don't sugarcoat.** The whole point of a premortem is to tell the user things they don't want to hear before reality does. If a plan has serious problems, say so directly.
- **The revised plan must be concrete.** Don't say "consider testing your pricing." Say "run a $47 pilot with 20 people before committing to the full $297 workshop." Every revision should be something the user can actually do this week.
- **Respect the minimum context threshold.** Running a premortem on insufficient context produces generic failures that waste the user's time. It's better to ask one more question than to produce a bad premortem.
- **This is not the LLM Council.** The council gives multiple perspectives on a decision right now. The premortem sends Claude into the future where the decision already failed and works backward to explain why. Different psychological mechanism, different output. If the user seems to want multiple perspectives rather than failure analysis, suggest the council instead.

## Example

User:

```text
Premortem this: I'm about to launch a $297 live workshop on how to use Claude for marketing teams. 50 seats. Targeting marketing managers at companies with 10-50 employees.
```

**Raw premortem identifies 6 failure reasons:**

1. Marketing managers at this company size need approval to spend $297 on professional development, adding friction you haven't accounted for
2. "Claude for marketing" is a tool-specific pitch in a market where most managers are still figuring out whether AI is relevant to them at all
3. The audience that actually buys might be solopreneurs, not team managers, creating a mismatch between content and attendees
4. Building a workshop for marketing teams requires demo environments with realistic marketing data and multi-seat setups, which takes 5 weeks of prep, not the 2 you budgeted
5. If 60% of attendees are solopreneurs, your reviews and case studies won't resonate with the marketing manager audience you need for future cohorts
6. At $297 with 50 seats, the max revenue is $14,850, which may not justify the prep time against other revenue opportunities

**6 agents go deep on each reason independently, producing failure stories, underlying assumptions, and early warning signs.**

**Synthesis:** Most likely failure is the audience mismatch: you're targeting people who need approval to spend $297, which adds friction you haven't accounted for. Most dangerous failure: attracting solopreneurs instead of team managers means your case studies and testimonials won't resonate with the actual target buyer for future cohorts, compounding the problem over time. Hidden assumption: you're assuming "marketing managers at 10-50 person companies" is a reachable audience, but these people don't self-identify that way and don't hang out in the same places. Revised plan: run a $47 pilot session for 20 people first. Use that to identify whether your actual buyers are team managers or solopreneurs, and build the full workshop for whoever actually shows up.
