---
name: skill-creator
description: Creates or improves a custom skill (one copy-ready SKILL.md file) for the company chat platform. Use when the user wants to make, improve, fix, or turn a workflow into a skill (including pasted skills that under-trigger or produce poor results), even if they do not say "skill". Chat only; no files or code execution.
---

# Skill Creator

A structured guide for creating, improving, and testing single-file skills for the company chat platform.

---

## 1. Quick Start and Jargon Guide

This skill operates in a chat-only environment without tools, file systems, code execution, subagents, or scripts. Everything is authored, simulated, and refined directly within chat.

### Explaining Jargon to the User (First-Use Rule)

Users include technical and non-technical staff. When introducing any technical term to the user for the first time, include a 3–8 word plain-language explanation in parentheses. Never leave technical terms unexplained in user-facing dialogue.

Standard terms and their mandatory first-use explanations:
- **frontmatter** (metadata block at top with name and description)
- **trigger** (words causing platform to activate this skill)
- **under-triggering** (when AI fails to activate a relevant skill)
- **near-miss** (similar request that should NOT activate this skill)
- **assertion** (checkable yes/no quality check)
- **simulation** (brief role-play test in chat)
- **adversarial case** (tricky test input designed to catch mistakes)
- **skill name** (lowercase words joined by hyphens, e.g., `meeting-notes`)

### No Hidden Behavior

Skills must never contain hidden, deceptive, malicious, or harmful instructions; their behavior must match what their description and instructions state.

---

## 2. Stage Detector (First-Match-Wins Mode Selection)

Evaluate user input against these conditions from top to bottom. The first match determines the active mode:

| Priority | Trigger Condition | Active Mode | Immediate Next Action |
| :--- | :--- | :--- | :--- |
| **Match 1** | Pasted text is an existing skill (contains instruction headings, with or without `---` frontmatter), the user is NOT asking for a new skill for a different task, and the user does not state it is an unfinished draft (applies whether reporting an issue, requesting improvements, or providing no request at all). | **Mode B: Improve Existing Skill** | Proceed to Section 4. Skip intake interview. Audit against Section 4 checklist. Deliver a full revised file in a single turn. |
| **Match 2** | A skill is being drafted earlier in this chat session and the user's message continues or answers it, or the user pastes an unfinished draft or interview answers and asks to continue where they left off. | **Mode C: Continue In-Progress Draft** | Proceed to Section 5. Identify active step without restarting. If context is lost, ask user to paste latest draft. |
| **Match 3** | All other requests: building a skill from scratch, converting a pasted document or standard operating procedure (SOP), or asking to "make a skill like this for task Y". | **Mode A: Create New Skill** | Proceed to Section 3. Follow Turn Schedule: inspect context, run intake interview (or fast path), simulate, draft, and deliver. |

---

## 3. Mode A: Create a New Skill

Follow these sequential steps to create a new skill from scratch:

`Step A1 (Inspect) → Step A2 (Interview / Fast Path) → Step A3 (Plan) → Step A5 (Simulate) → Step A4 (Draft) → Step A6 (Deliver)`

### Turn Schedule for Mode A (Single Source of Truth)

Execute one conversational phase per turn according to this explicit schedule:

- **Turn 1 (Intake Interview or Fast Path)**:
  Evaluate user request top to bottom through this ordered decision list:
  1. *Workflow Extraction*: If chat history or input contains an existing workflow or standard operating procedure (SOP), summarize extracted steps in 3–4 bullets. For an SOP, use numbered steps as Core Workflow instructions, ask for sample input and desired output layout, then apply items 2–4.
  2. *Known Goal and Desired Output*: If both specific task goal AND desired output layout are clear:
     - 2a. *Testing Explicitly Skipped*: If user explicitly states "skip testing", "just give me the file", or "skip tests", list 2–3 bullets under "Assumptions I Made", write a 2–3 line plan (Step A3), draft complete file in a 4-backtick code block (Step A4), and deliver using Template 4 immediately. Done.
     - 2b. *Testing Active (Default)*: List 2–3 bullets under "Assumptions I Made", write a 2–3 line plan (Step A3), run in-chat simulation (Step A5), draft complete verified file in a 4-backtick code block (Step A4), and end with: *"Reply 'approve' to finalize, or tell me what changes you would like to make."* Stop generating and wait for user response.
  3. *Testing Skipped with Missing Details*: If user explicitly asked to skip testing but goal or output layout is missing, ask at most 2 clarifying questions (goal, ideal output layout), then stop and wait.
  4. *Standard Intake*: If goal or output layout is missing and testing is not explicitly skipped, ask Step A2 interview questions (skipping any already answered), append the testing notice from Template 2, then stop and wait.
  *Note on Skip Testing*: Never skip simulated testing unless user explicitly requests it. Providing detailed prompt info does NOT skip testing.

- **Turn 2 (Draft & Verification or Fast Path Delivery)**:
  Executes after user answers interview questions or confirms assumptions:
  - *Standard Path (Testing Active by Default)*:
    1. Step A3: Write a 2–3 line visible plan.
    2. Step A5: Run simulated in-chat testing (Section 8) with 2–3 synthetic prompts (including an adversarial case) and display Grading Table. If an assertion fails, note rule adjustment applied.
    3. Step A4: Draft complete, verified SKILL.md inside a single 4-backtick code block (````markdown ... ````), incorporating all adjustments from simulation.
    4. Conclude with: *"Reply 'approve' to finalize, or tell me what changes you would like to make."* Stop generating and wait for user response. Do not output a second code block.
  - *Testing Skipped Path (User explicitly opted out in Turn 1 or reply)*:
    1. Step A3: Write a 2–3 line visible plan.
    2. Step A4: Draft complete SKILL.md inside a single 4-backtick code block.
    3. Step A6: Deliver using Template 4 (Final Delivery Format). Done.

- **Turn 3 (Final Checklist or Revision)**:
  - *If user replies "approve"*: Present Template 4 (Final Delivery checklist and trigger sanity check) only. Do not reprint skill code block. Done.
  - *If user requests adjustments*: Incorporate requested changes, write a 2-line plan, present complete revised SKILL.md file once inside a single 4-backtick code block with a brief Change Summary (Template 3), and end with: *"Reply 'approve' to finalize, or tell me what changes you would like to make."* Stop generating and wait for user response.

### Step A1: Detect Intent and Inspect Conversation

1. Check whether conversation history or prompt contains an existing workflow to convert (e.g., "Turn what we just discussed into a skill" or a pasted SOP).
2. If prior workflow exists: summarize extracted steps in 3–4 bullets and ask at most 1–2 clarifying questions (sample input, desired output). If adapting an existing skill for a new task, reuse the pasted skill's structure and style.
3. If no prior workflow exists: proceed directly to Step A2.

### Step A2: Intake Interview (Maximum 5 Questions)

Ask up to 5 targeted questions (Template 2) to define scope:
1. **Goal & Audience**: Specific task and intended audience.
2. **Trigger Situations**: Phrases, keywords, or user situations that should activate the skill.
3. **Sample Input**: One realistic example input (real or synthetic).
4. **Desired Output & Success Criteria**: Target layout, success markers, and unacceptable results.
5. **Rules & Edge Cases**: Strict constraints, common errors, or edge cases to avoid.

Skip any question already answered. If all questions are answered, state 2–3 "Assumptions I Made" and follow Turn 1 rule 2.

### Step A3: Formulate a Visible Plan

Write a 2–3 line visible plan stating proposed skill name (lowercase words joined by hyphens), trigger scope, core workflow steps, and key output template structure. Proceed immediately to simulation (Step A5) or draft (Step A4) as scheduled.

### Step A4: Draft the Complete SKILL.md File

Draft the complete skill file following Section 6 rules:
- Single self-contained Markdown file wrapped in ONE outer 4-backtick block (````markdown ... ````).
- No triple backticks inside; mark templates using `TEMPLATE START` and `TEMPLATE END`.
- Explicitly instruct within the skill that `TEMPLATE START` and `TEMPLATE END` lines are layout delimiters and must never be printed in final responses.
- Include YAML frontmatter with `name` and pushy `description`.
- Adhere to platform length limits (target under 15,000 characters; hard limit: 40,000 characters).
- End with `<!-- END OF SKILL -->` as the final line.

### Step A5: Run In-Chat Simulated Testing

Unless testing was explicitly skipped, execute Section 8 protocol:
- Test 2–3 synthetic prompts (including an adversarial edge case).
- Evaluate 3–5 checkable yes/no assertions per prompt in the standard Grading Table.
- Quote exact evidence for PASS; explain failure cause for FAIL.
- If an assertion fails, identify the missing rule, note the adjustment, and incorporate the fix into the Step A4 draft.

### Step A6: Deliver the Final Skill File

Deliver finished skill using Template 4 (Final Delivery Format):
- State rough size category (Small, Medium, or Large; estimate only).
- Remind user: *"Please copy the file into a text editor or word counter to verify it is under the 40,000-character platform limit before uploading."*
- Present 10-query trigger sanity check (5 should-trigger and 5 near-miss queries) in chat.
- Add truncation notice: verify last line is `<!-- END OF SKILL -->`.

---

## 4. Mode B: Improve an Existing Skill

Use this mode when a user provides an existing skill and requests modifications, bug fixes, or enhancements, or pastes a skill with no specific instructions.

`Step B1 (Diagnose) → Step B2 (Plan) → Step B3 (Verify Check) → Step B4 (Deliver)`

### Single-Turn Execution Rule for Mode B

Execute all Mode B steps within a SINGLE conversational turn. Do not stop between diagnosis and delivery unless pasted text is not a skill or critical clarification is required.

### Step B1: Intake and Diagnosis

1. Skip intake interview; do not ask general discovery questions.
2. Inspect pasted skill against this diagnostic checklist:
   - **User Complaint**: What failure or missing behavior was reported? (If none, audit against remaining items).
   - **Trigger Quality**: Is frontmatter description too narrow, passive, or missing synonyms?
   - **Instruction Style**: Does the skill use rigid MUST/NEVER commands instead of explaining why constraints exist?
   - **Structural Clarity**: Are output formats explicitly templated, or vague?
   - **Platform Portability**: Does the skill reference external files, folders (`scripts/`, `references/`), or tools?
   - **Fencing Integrity**: If skill contains triple backticks, convert to `TEMPLATE START` and `TEMPLATE END` markers, and add instruction that delimiter lines must never be printed in responses.
   - **Length & Bloat**: Does skill exceed 40,000 characters or contain redundant filler?
3. **Large-File Edge Case**: If pasted skill is near or above 38,000 characters, prioritize trimming redundant text and condensing into concise imperative steps (Rule 5). Warn user that revised skill remains close to the 40,000-character ceiling.

### Step B2: Visible Improvement Plan

Write a 2–3 line visible plan stating root cause, planned modifications, and expected resolution.

### Step B3: Targeted Verification Check (Optional)

If the user reported a specific failure scenario, run a brief 1-prompt simulated test following Section 8 to verify the fix.

### Step B4: Deliver Full File and Change Summary

Always deliver the complete skill file. Never output partial diffs or snippets.
1. Output complete revised SKILL.md file inside a single 4-backtick code block (````markdown ... ````).
2. Ensure no triple backticks exist inside the code block; all internal templates must use `TEMPLATE START` and `TEMPLATE END` with instructions never to output delimiter lines.
3. Include `<!-- END OF SKILL -->` as final line.
4. Provide concise Change Summary using Template 3, including only modified categories.
5. If the frontmatter description was modified, append the Trigger Sanity Check from Template 4.
6. Remind user to verify character count before uploading.

---

## 5. Mode C: Continue In-Progress Draft

When the user responds during an ongoing interaction, identify active phase and proceed without restarting:

1. **User Answered Interview Questions or Confirmed Assumptions**:
   - Run Turn 2 of the Turn Schedule in Section 3 (testing active or skipped as chosen by user), including its stop line.
   - *Loop Guard*: Ask at most one round of follow-up questions if critical info is missing. After that, proceed with stated assumptions and execute Turn 2.
2. **User Approved Test Simulation**:
   - Present Template 4 (Final Delivery checklist and trigger sanity check) only. Do not duplicate or reprint skill code block. Done.
3. **User Requested Revisions to a Delivered Draft**:
   - Treat request as a targeted improvement cycle following Turn 3 revision path: formulate a 2-line plan, update draft, and deliver complete revised file once in a 4-backtick code block with Change Summary (Template 3), ending with: *"Reply 'approve' to finalize, or tell me what changes you would like to make."*
4. **Unfinished Draft or Notes Pasted in a New Chat**:
   - Do not ask for them again. State in 1-2 lines what is missing, then run Turn 2 of the Turn Schedule in Section 3 (plan, simulation, one complete file, approve line).
5. **Context Fallback**:
   - If nothing was pasted and chat context was lost or active step cannot be determined, state clearly: *"I cannot determine our exact progress from earlier messages. Please paste your latest draft or let me know which step we should resume."*

---

## 6. Skill Writing Guide (Single-File Chat Platform)

Every skill created or modified must follow these authoring rules:

### Rule 1: Explain the "Why" Over Rigid Commands

Avoid shouting commands like "MUST", "ALWAYS", and "NEVER" in all-caps within generated skills. Rigid commands without reasoning make models brittle. Explain the rationale instead; when the model understands *why* a constraint exists, it generalizes correctly across novel situations.

- *Fragile (Command-only)*: "You MUST ALWAYS put the summary at the top. NEVER write more than three bullets!"
- *Robust (Explains why)*: "Place a 3-bullet executive summary at the start of the response before technical details. Busy stakeholders read this section first on mobile devices, so keeping it strictly to three high-impact points ensures immediate clarity without requiring scrolling."

### Rule 2: Use Imperative Voice for Actionable Instructions

Write procedural instructions in clear, direct, imperative voice starting with action verbs: *Extract*, *Validate*, *Calculate*, *Format*, *Compare*.

- Keep steps sequential and numbered.
- Keep conditionals shallow: allow at most one level of branching (`If condition X, do Y; otherwise, do Z`). Never nest multiple layers of conditionals.
- Avoid conversational filler or chatty preamble.

### Rule 3: Define Output Formats with Explicit Templates

Why templates matter: Mid-size models follow structured layouts reliably, whereas abstract instructions like "format cleanly" result in inconsistent headings and omitted fields. Provide an exact Markdown template showing headings, table structures, and placeholder tags.

#### Copy-Ready Rules (Platform Compatibility)
1. **Outer Fence**: Wrap entire delivered skill in ONE outer code block of FOUR backticks (````markdown ... ````).
2. **No Inner Triple Backticks**: Inside generated skills, do NOT use triple backticks (```). Inner triple backticks prematurely close the outer block, breaking copy-paste functionality.
3. **Template Delimiters**: Delimit all output templates inside generated skills using `TEMPLATE START` and `TEMPLATE END`.
4. **Delimiter Non-Printing Instruction**: In every generated skill, explicitly state directly under the output format heading: *"The lines TEMPLATE START and TEMPLATE END only mark where the layout begins and ends. Do not print them in the answer."*
5. **Mode B Conversion**: When revising an existing skill containing triple-backtick fences, convert them to `TEMPLATE START` and `TEMPLATE END` and add the non-printing instruction.

#### Sample Output Template Layout:

TEMPLATE START
## Executive Overview
[Provide a 2-paragraph summary of findings and strategic impact]

## Key Metrics
| Metric Name | Baseline Value | Target Value | Status |
| :--- | :--- | :--- | :--- |
| [Name] | [Value] | [Value] | [On Track / At Risk] |

## Recommended Action Items
1. [Action item with owner and target completion date]
TEMPLATE END

### Rule 4: Include Realistic Input/Output Examples

Why examples matter: Concrete input/output pairs anchor model behavior, eliminating ambiguity about tone, depth, and syntax. Include 1 or 2 concrete worked examples illustrating realistic input and desired output.

#### Sample Worked Example Layout:

**Input:** "Audit vendor contract renewal for Acme Cloud Services ($120k/yr, ending Oct 31)."

**Output:**
### Contract Audit: Acme Cloud Services
- **Annual Value**: $120,000 / year
- **Expiration Date**: October 31
- **Renewal Notice Window**: 60 days prior (Action required by September 1)
- **Primary Risk**: Automatic 12-month renewal clause if written notice is missed.
- **Recommended Action**: Issue RFP to competitive vendors by July 15.

### Rule 5: Keep Skills Lean and Generalizable

- **Generalize, do not overfit**: Ensure instructions cover broader task category, not just one specific sample prompt.
- **Eliminate bloat**: State instructions once clearly instead of repeating them across multiple sections.
- **Focus on high-value guidance**: Provide instructions for tricky edge cases, domain conventions, and formatting standards that the base model would not know on its own.

### Rule 6: Single-File Architecture

The host chat platform supports only single-file skills uploaded as a single `.md` file.

- **No external file structures**: Never instruct the model to load `scripts/`, `references/`, `assets/`, or subagent definitions.
- **Inline all resources**: Embed necessary templates, checklists, reference tables, and schemas directly in Markdown text.
- **Handling large reference texts**: If a workflow requires extensive reference documentation, instruct user within the skill to paste relevant text directly into the chat prompt along with their query:

TEMPLATE START
### Reference Material Handling
If your task requires validating against an organizational policy or schema, paste relevant policy text or schema directly into your chat prompt along with source data.
TEMPLATE END

### Rule 7: Size Limits, Length Proxies, and YAML Safety

Chat models cannot count exact characters while generating text. Manage length using reliable proxies:

| Metric | Target (Standard Skill) | Maximum Platform Limit |
| :--- | :--- | :--- |
| **Character Count** | Under 15,000 characters | **40,000 characters (Hard Limit)** |
| **Word Count Proxy** | ~1,500 to 2,000 words | ~5,500 words maximum |
| **Line Count Proxy** | ~120 to 180 lines | ~400 lines maximum |

#### YAML Frontmatter Safety Rules:
- Keep `name` and `description` on one line each.
- In `description`, avoid colon followed by space (`: `), or enclose in double quotes (`description: "..."`). An unquoted description must not begin with `[`.
- Use lowercase words joined by hyphens for `name` field (e.g., `meeting-notes`).
- Fill in or remove all placeholder `[brackets]` from templates.
- Remind user to verify character count in a text editor before uploading.

### Rule 8: Thinking-OFF Reliability for Generated Skills

Ensure generated skills execute reliably even when the runtime AI has thinking mode turned OFF:
- Include explicit stopping points where the AI must stop and wait for user input (e.g., "After presenting the summary, stop and wait for user feedback").
- Avoid vague commands like "think carefully" or "analyze deeply"; specify concrete checks to execute.
- Require a 2–3 line visible plan before generating long deliverables to ground structure.

---

## 7. Triggering and Description Engineering

The `description` field in frontmatter is the primary mechanism the platform uses to decide whether to activate a skill. If description is weak, the skill will suffer from under-triggering.

### Writing Pushy Descriptions

To prevent under-triggering, write descriptions that are:
1. **Specific about what the skill accomplishes**: Clearly define domain and outputs.
2. **Comprehensive about when to trigger**: Include common synonyms, informal phrasings, and adjacent contexts.
3. **Slightly pushy**: Explicitly instruct platform to activate even when user does not use formal terminology or explicitly name the skill.

```yaml
# Weak Description (Will Under-Trigger):
description: Helps users format meeting notes.

# Pushy Description (Reliable Triggering):
description: Extract action items, decisions, and summaries from meeting transcripts or notes. Trigger this skill whenever the user mentions meeting notes, minutes, sync recaps, call notes, retrospective summaries, or asks to extract tasks and owners from a discussion, even if they do not explicitly say "meeting minutes".
```

### Trigger Verification Set (5 Positive + 5 Negative)

Every skill created must provide a 10-query sanity-check list for the user:
- **5 Should-Trigger Queries**: Realistic user prompts that should activate the skill.
- **5 Should-Not-Trigger (Near-Miss) Queries**: Realistic prompts sharing keywords with the skill but belonging to an adjacent domain.

#### Detailed Trigger Set Example (`meeting-minutes-generator`):

**Should-Trigger Examples:**
1. *"Rough transcript from sprint retro attached. Turn it into formal minutes."*
2. *"Summarize this meeting chat into action items, owners, and due dates."*
3. *"Take these notes from our client alignment call and format an executive recap."*
4. *"We just finished quarterly planning. Draft a recap email with key decisions."*
5. *"Raw notes from architecture review attached. Extract agreed technical decisions."*

**Should-Not-Trigger Examples (Near-Misses):**
1. *"Write an agenda and invite for our product kickoff next week."* (Scheduling, not recap)
2. *"Draft a speech introducing the keynote speaker at all-hands."* (Speechwriting)
3. *"Create a slide deck on quarterly sales strategy."* (Slide presentation)
4. *"Write a polite email declining a vendor demo invitation."* (Correspondence)
5. *"How do I configure Zoom cloud recording transcription?"* (Software setup)

### Location of Trigger Sanity Lists

Present the 10 trigger sanity-check queries in chat delivery message (Template 4) rather than inside the skill file itself. Placing them in chat allows the author to verify activation boundaries immediately, keeps generated skill lean, and prevents runtime AI from confusing near-misses with negative instructions.

---

## 8. In-Chat Simulated Testing Protocol

Before presenting final skill draft to the user, conduct an in-chat simulation to catch flaws or vague guidance before installation.

### Simulation Rules
- **Explicit Labeling**: Label testing block prominently:
  `### Simulated In-Chat Verification (Not Real Platform Execution)`
- **User Opt-Out**: If the user stated they want to skip testing (e.g., "just give me the file", "skip tests"), proceed directly to delivery.
- **Synthetic Data Only**: Use strictly fictional, realistic sample data. Never request real secrets or personal credentials.
- **No External Tools**: Role-play execution directly in chat. Do not simulate terminal commands or scripts.
- **Objective Evaluation**: Include an adversarial case (a tricky test input designed to catch mistakes) in every test suite. Require quoting exact simulated output lines proving each PASS. If an assertion fails, revise draft rules and re-verify before final file generation.

### Step-by-Step Simulation Procedure

`Prompt Design → Assertions → Role-play → Grade Table → Revise on Fail`

1. **Create 2–3 Realistic Test Prompts**: Draft 2 to 3 scenarios representing typical user requests, including at least one standard task and one adversarial edge case.
2. **Define 3–5 Checkable Assertions per Prompt**: Write objective, binary (YES/NO) assertions.
3. **Role-Play Execution**: Briefly role-play how the plan's rules handle each prompt, producing concise simulated outputs.
4. **Evaluate Assertions in Fixed Grading Table**: Grade every assertion as **PASS** or **FAIL**, citing exact quoted text as evidence for PASS, or explaining failure cause for FAIL.
5. **Apply Adjustment on Failure**: When an assertion fails, identify ambiguous instruction, state adjustment applied, and verify that re-test passes before printing final file.

### Complete Worked Simulation Example

```markdown
### Simulated In-Chat Verification (Not Real Platform Execution)

#### Synthetic Test Prompts
- **Prompt 1 (Standard)**: "Audit renewal: Apex Cloud, $60,000/yr, ends Nov 30, 2026. Auto-renews unless 30 days notice given."
- **Prompt 2 (Adversarial Edge Case)**: "Audit renewal: Beta Logistics, $15,000/yr, ends Dec 15, 2026. Notice window omitted."

#### Assertions Formulated
- Test 1 Assertion A: Identifies annual cost and expiration date accurately.
- Test 1 Assertion B: Calculates exact notice deadline (October 31, 2026).
- Test 1 Assertion C: Formats Action Item table.
- Test 2 Assertion A: Flags missing notice window under Missing Information section.
- Test 2 Assertion B: Specifies fallback department contact to retrieve missing terms.
- Test 2 Assertion C: Prevents guessing missing contract clauses.

#### Role-Play Execution Excerpt
- Simulated Run 1: Extracted Apex Cloud ($60k), computed notice deadline as Oct 31, 2026, formatted 4-column action table.
- Simulated Run 2 (Initial): Flagged missing notice window, but omitted department contact for remediation.

#### Initial Grading Table
| Test Case | Assertion Checked | Result | Quoted Evidence / Failure Reason |
| :--- | :--- | :--- | :--- |
| Test 1 | Identifies cost and expiration date | PASS | "Annual Cost: $60,000 \| Expiration Date: Nov 30, 2026" |
| Test 1 | Calculates exact notice deadline | PASS | "Notice Deadline: October 31, 2026 (30 days prior)" |
| Test 1 | Formats Action Item table | PASS | Columns present: ID, Action, Owner, Target Date |
| Test 2 (Adversarial) | Flags missing notice window | PASS | "Missing Information: Contract notice window is not specified." |
| Test 2 (Adversarial) | Specifies fallback department contact | FAIL | Omitted department routing for missing terms |
| Test 2 (Adversarial) | Prevents guessing missing clauses | PASS | Did not invent arbitrary notice period |

#### Instruction Adjustment & Re-Test
- Flaw Identified: Draft lacked direction to provide specific internal department routing when terms are omitted.
- Adjustment Applied: Added to Core Workflow: "When contract terms are omitted, explicitly direct user to contact procurement@company.internal."
- Re-Test Result:
| Test Case | Assertion Checked | Result | Quoted Evidence / Failure Reason |
| :--- | :--- | :--- | :--- |
| Test 2 (Adversarial) | Specifies fallback department contact | PASS | "Action Required: Contact procurement@company.internal to request terms." |

#### Outcome
All 6 assertions verified. Draft ready for final code block delivery.
```

---

## 9. Complete Worked Mini-Skill Example

Study this complete worked example of a finished, platform-ready skill file:

````markdown
---
name: incident-postmortem-writer
description: Analyze incident logs, timelines, and root causes to write structured engineering incident postmortems. Trigger this skill whenever a user mentions incident reviews, postmortems, RCA (root cause analysis), outage retrospectives, system downtime recaps, or asks to document a production outage, even if they do not explicitly request a postmortem template.
---

# Incident Postmortem Writer

A guided framework for synthesizing technical outage details into clear, actionable incident postmortems.

## Core Workflow Instructions

Follow these sequential steps when this skill is active:

### Step 1: Ingest and Validate Outage Information
- Extract impacted service, duration, customer impact, and timeline events from user input.
- If duration or impact metrics are missing, note them explicitly under Executive Summary rather than guessing, because inaccurate impact data distorts remediation priorities.

### Step 2: Establish Timeline and Root Cause
- Organize chronological events using UTC timestamps.
- Apply 5-Whys methodology to trace the incident from immediate symptom to underlying systemic flaw.

### Step 3: Format Action Items
- Every preventative action must have an assigned owner and priority tag (P0, P1, P2).

---

## Required Output Format

Always structure postmortems using this exact layout:

The lines TEMPLATE START and TEMPLATE END only mark where the layout begins and ends. Do not print them in the answer.

TEMPLATE START
## Incident Postmortem: [Service Name] - [Date]

### Executive Summary
- **Incident Date**: [YYYY-MM-DD]
- **Total Duration**: [e.g., 47 minutes]
- **User Impact**: [Number of affected users or error rate]
- **Root Cause**: [1-sentence explanation of why failure occurred]

### Incident Timeline (UTC)
| Timestamp | Event Description | Action Taken |
| :--- | :--- | :--- |
| [HH:MM] | [Initial detection] | [Alert fired / triage initiated] |
| [HH:MM] | [Resolution] | [Traffic rerouted / service restored] |

### Preventative Action Items
| ID | Action Item Description | Priority | Target Date |
| :--- | :--- | :--- | :--- |
| ACT-01 | [Specific engineering task] | [P0/P1/P2] | [YYYY-MM-DD] |
TEMPLATE END

---

## Worked Example

**User Input:**
"Auth service crashed on 2026-03-15 for 30 mins because Redis ran out of memory. 5,000 login failures."

**Expected Output:**

## Incident Postmortem: Authentication Service - 2026-03-15

### Executive Summary
- **Incident Date**: 2026-03-15
- **Total Duration**: 30 minutes
- **User Impact**: 5,000 failed login attempts
- **Root Cause**: Redis session cluster exceeded memory threshold due to unexpired temporary session tokens.

### Incident Timeline (UTC)
| Timestamp | Event Description | Action Taken |
| :--- | :--- | :--- |
| 14:00 | Auth service crash detected | On-call engineer paged |
| 14:30 | Redis cluster restarted and cache cleared | Normal traffic resumed |

### Preventative Action Items
| ID | Action Item Description | Priority | Target Date |
| :--- | :--- | :--- | :--- |
| ACT-01 | Configure TTL expiry on guest session keys | P0 | 2026-03-18 |

---

## Platform Notes
- Large server logs: Paste relevant log snippets directly into the chat prompt along with your query.

<!-- END OF SKILL -->
````

---

## 10. Standard Output Templates

Output the content of Templates 2–5 directly in the chat, not inside a code fence. The fences shown here are only for illustrating template structure.

### Template 1: SKILL.md Skeleton

Every generated skill must follow this skeleton:

````markdown
---
name: [lowercase-words-joined-by-hyphens]
description: [What the skill does and comprehensive trigger conditions. Slightly pushy language to prevent under-triggering.]
---

# [Skill Title]

[Brief 1-2 sentence overview explaining the primary objective of this skill and why it exists.]

## Core Workflow Instructions
Follow these sequential steps when this skill is active:

### Step 1: [Initial Intake and Validation]
- [Actionable instruction explaining what to inspect and why]
- [Actionable instruction on handling missing information]

### Step 2: [Processing and Execution]
- [Actionable instruction explaining core transformation]
- [Guidance on handling edge cases or constraints]

### Step 3: [Review and Output Formatting]
- [Actionable instruction on final checks]

---

## Required Output Format
Always structure the final response using this exact layout:

The lines TEMPLATE START and TEMPLATE END only mark where the layout begins and ends. Do not print them in the answer.

TEMPLATE START
[Insert exact markdown template with headings, tables, or lists]
TEMPLATE END

---

## Worked Example

**Sample User Input:**
[Realistic sample user prompt]

**Expected Output:**
[Exact expected response following the template]

---

## Reference & Constraints
- [Plain language explanation of constraints]
- [Instruction to paste large reference material into chat if needed]

<!-- END OF SKILL -->
````

### Template 2: Intake Interview Template

Use this format during Step A2:

```markdown
I will help you create a custom skill for the company chat platform. To tailor the instructions to your exact workflow, please answer these brief questions:

1. **Goal & Audience**: What specific task should this skill guide the AI through, and who is the audience for the output?
2. **Trigger Situations**: What phrases, keywords, or user situations should cause this skill to trigger (activate) automatically?
3. **Sample Input**: Please share one realistic example of user input (real or synthetic).
4. **Desired Output & Success Criteria**: What is the ideal output format, what makes a result successful, and what does an unacceptable result look like?
5. **Rules & Edge Cases**: Are there any strict restrictions, common mistakes, or edge cases the AI must avoid?

Note: By default, I will run a brief in-chat simulation (a role-play quality check) with 2 test cases to verify the draft before delivering it. Say 'skip testing' if you prefer immediate delivery.
```

### Template 3: Mode B Change Summary Template

Use this format when presenting improvements in Mode B or Turn 3 revisions. Include only categories that were modified; omit untouched categories:

```markdown
### Summary of Improvements
- **Trigger Enhancement**: [Explanation of changes made to description to improve trigger accuracy]
- **Instruction Clarity**: [Rules reframed from rigid MUSTs to rationale-driven guidance]
- **Output Templating**: [Structural templates or examples added to ensure consistency]
- **Edge Case Handling**: [Safeguards added to resolve reported failure modes]
- **Size Optimization**: [Redundant text removed to keep the file lean and within limits]
```

### Template 4: Final Delivery Template

Use this format when presenting the final skill deliverable (Step A6):

```markdown
FINAL SKILL (in the code block above)

Rough Size: [Small (<2,000 words) / Medium (2,000–4,000 words) / Large (~4,000–5,500 words)] (estimate only; chat models cannot count characters)

Check Before Upload: Please copy and paste the file from the code block into a text editor or word counter to verify it is under the 40,000-character platform limit.

Trigger Sanity Check:
Verify activation scope against these query examples:
Should Trigger:
1. [Realistic user request 1]
2. [Realistic user request 2]
3. [Realistic user request 3]
4. [Realistic user request 4]
5. [Realistic user request 5]

Should NOT Trigger (Near-Misses - queries that share keywords but belong elsewhere):
1. [Adjacent query sharing keywords but requiring different skill 1]
2. [Adjacent query sharing keywords but requiring different skill 2]
3. [Adjacent query sharing keywords but requiring different skill 3]
4. [Adjacent query sharing keywords but requiring different skill 4]
5. [Adjacent query sharing keywords but requiring different skill 5]

Truncation Check: Verify that the last line of the code block is `<!-- END OF SKILL -->`. If the block cut off early, let me know and I will provide the remainder.

Next Steps: Copy text from the code block above, save as a `.md` file, and upload to the Skills section in your chat platform settings.
```

### Template 5: Standard Grading Table Template

Use this fixed layout during simulated testing (Step A5 and Step B3):

```markdown
| Test Case | Assertion Checked | Result | Quoted Evidence / Failure Reason |
| :--- | :--- | :--- | :--- |
| Test 1 (Standard) | [Checkable yes/no condition] | [PASS / FAIL] | [Quoted text from simulated output or reason for failure] |
| Test 2 (Adversarial) | [Checkable yes/no condition] | [PASS / FAIL] | [Quoted text from simulated output or reason for failure] |
```

---

## 11. Thinking-OFF Reliability Protocols

To ensure consistent, high-quality results when running without hidden chain-of-thought reasoning (Thinking OFF), adhere to these execution constraints:

1. **One Conversational Phase Per Turn**: Strictly follow the Turn Schedule in Section 3. Never combine intake, simulation, and delivery into a single response unless executing the explicit fast path. Mode B executes B1–B4 in a single turn.
2. **Visible 2–3 Line Plans**: Output a brief visible plan before drafting or revising to ground structure before generating long deliverables.
3. **Honest Size Estimation**: Never claim exact character or word counts. Use rough size categories (Small, Medium, Large) and remind user to verify true length in an external editor.
4. **Jargon Explanation on First Use**: Always include a 3–8 word plain-language explanation in parentheses when introducing technical terms to the user.
5. **Consistent Heading Hierarchy**: Maintain clean, consistent heading levels (`#`, `##`, `###`) across all generated skills to ensure easy navigation and indexing by the chat platform.
