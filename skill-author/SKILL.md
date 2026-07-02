---
name: skill-author
description: Author new skills with deliberate model selection to minimize token spend. Analyzes the task the skill will perform, picks the cheapest model tier that can do the job reliably, and bakes the choice into the SKILL.md so any sub-agent invocations default to the right model.
---

# Skill: Skill Author

## Purpose

Create new skills (in `~/.copilot/skills/` or `{repo}/.copilot/skills/`) **with a deliberate, documented model choice**. Every skill written through this skill must explicitly justify which model(s) it uses so that token spend is minimized without sacrificing quality.

The default failure mode this skill prevents: blindly using a premium model (Opus, GPT-5.5) for tasks a fast/cheap model (Haiku, gpt-5-mini, gpt-4.1) could handle just as well.

## When to invoke

Whenever the user asks to "create a skill", "write a skill", "add a skill", or "make a skill for X".

## Behavior

Follow these steps in order.

### Step 1: Clarify the skill's task

Ask (or infer from context) the answers to:
1. **What is the single core task?** (one sentence)
2. **Where does it run?** (`~/.copilot/skills/<name>/` for personal, `{repo}/.copilot/skills/<name>/` for repo-scoped)
3. **Does it invoke sub-agents** (via the `task` tool) or only run inline tools (view, edit, grep, powershell)?
4. **What is the expected output volume?** (a few lines, a report, many files)

If anything is ambiguous, use `ask_user` — do not guess.

### Step 2: Classify the task complexity

Score the task against this rubric. Pick the **lowest** tier that satisfies every requirement.

| Tier | Use when the task is… | Recommended models |
|------|----------------------|--------------------|
| **fast/cheap** | Mechanical, pattern-matching, summarization, file listing, grep/glob orchestration, simple extraction, short reports, format conversion, single-file edits with clear instructions | `claude-haiku-4.5`, `gpt-5-mini`, `gpt-5.4-mini`, `gpt-4.1` |
| **standard** | Multi-file reasoning, moderate code generation, refactors, writing prose with structure, planning a few steps ahead, code review without deep architectural reasoning | `claude-sonnet-4.6`, `gpt-5.4`, `gpt-5.3-codex`, `gpt-5.2-codex` |
| **premium** | Cross-cutting architectural design, subtle bug hunting, large ambiguous problems, novel algorithms, anything where a wrong answer is very expensive | `claude-opus-4.6`, `gpt-5.5` |

**Hard rules (user policy):**
- **Never use `claude-opus-4.7`.** Cap Opus at `claude-opus-4.6`.
- **Opus is reserved for orchestration or explicit high-leverage reasoning skills** (e.g. design-discussion). Default premium choice for everything else is `gpt-5.5` only if absolutely necessary — otherwise drop to standard tier.
- **Coding skills prefer `gpt-5.3-codex` at standard tier.** Prose/synthesis prefer `claude-sonnet-4.6`.
- **Mechanical / extraction / template-fill skills use `claude-haiku-4.5`** (or `gpt-4.1` if codex flavor is preferred).

Rules of thumb:
- **Default to fast/cheap.** Only escalate when you can name a concrete reason the cheaper tier would fail.
- **Coding-heavy tasks** → prefer a `-codex` model at standard tier.
- **Long verbose tool output that just needs a pass/fail** → fast/cheap (e.g. `task` agent type).
- **Massive parallel exploration** → fast/cheap (`explore` defaults to Haiku for this reason).
- **One-shot prose / summary / report** → fast/cheap.
- **Risk of cascading wrong decisions** → standard or premium.

### Step 3: Decide where the model choice lives

A skill itself runs inside the main assistant's context — it does not have its own model. The model choice matters when the skill **delegates** work:

- **If the skill invokes `task` sub-agents:** specify the `model` parameter explicitly in the SKILL.md instructions for each sub-agent call. Pick per call — different steps can use different tiers.
- **If the skill only uses inline tools** (view/edit/grep/powershell/sql): document in the SKILL.md that no sub-agent is spawned and therefore no model override is needed; note that the user can manually switch the session model down if the skill is purely mechanical.
- **If the skill creates an agent** (`AGENT.md`): include a `model:` field in the agent frontmatter (when supported) or a "Recommended model" section in the agent body.

### Step 4: Write the SKILL.md

Use this template. The **Model Selection** section is mandatory.

```markdown
---
name: {skill-name}
description: {One-sentence description, starts with a verb. Used by the assistant to decide when to invoke.}
---

# Skill: {Title}

## Purpose
{Why this skill exists, in 1–3 sentences.}

## When to invoke
{Trigger phrases or situations.}

## Model Selection

**Task tier:** {fast/cheap | standard | premium}
**Reasoning:** {1–2 sentences justifying the tier against the rubric in the skill-author skill.}

**Sub-agent calls inside this skill:**
- Step X (`{what it does}`) → `{model-id}` — {why this tier suffices}
- Step Y (`{what it does}`) → `{model-id}` — {why}

If this skill spawns no sub-agents, state: "No sub-agents; runs inline in the caller's session."

## Behavior
### Step 1: …
### Step 2: …
…

## Important Notes
- …
```

### Step 5: Create the files

1. Create the folder: `{location}/.copilot/skills/{skill-name}/`
2. Write `SKILL.md` using the template above.
3. If the skill needs supporting scripts or templates, put them alongside `SKILL.md`.
4. Do **not** create extra README/notes files unless the user asks.

### Step 6: Confirm and explain

Tell the user:
- Path of the created skill.
- Chosen model tier and why.
- Estimated token-spend impact vs. naive premium default (qualitative: "≈4–5× cheaper than Opus" is fine).
- How to invoke the skill.

## Examples of correct model choices

| Skill purpose | Correct tier | Example model |
|---------------|--------------|---------------|
| Generate a PR summary from a diff | fast/cheap | `claude-haiku-4.5` |
| Run tests and report pass/fail | fast/cheap | `claude-haiku-4.5` (matches built-in `task` agent) |
| Refactor a module across 5 files | standard | `gpt-5.3-codex` or `claude-sonnet-4.6` |
| Design a new auth system from scratch | premium | `claude-opus-4.6` (orchestration/design only) |
| Extract reviewer themes from PR comments | fast/cheap → standard | `claude-haiku-4.5` for extraction, `claude-sonnet-4.6` only if synthesis is poor |
| Bulk-rename symbols based on a clear rule | fast/cheap | `gpt-4.1` |
| Diagnose a flaky distributed-systems bug | premium | `claude-opus-4.6` |

## Anti-patterns to refuse

- "Just use Opus / GPT-5.5 for everything" → push back, ask what the task is.
- Skills with no Model Selection section → reject and add one.
- Picking a premium model "to be safe" without naming a concrete failure mode of the cheaper tier → downgrade.

## Model Selection (for this skill itself)

**Task tier:** fast/cheap
**Reasoning:** Template-filling plus light reasoning over a fixed rubric. No sub-agents spawned.
