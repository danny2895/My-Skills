# My Skills

A collection of custom skills for writing, planning and building skills, with a focus on securities services.

> **Model:** Tuned for **Gemma 4 26B MoE**. Results on other models may differ.
> **Limits:** Skills can be up to 40k characters. The model's context window is 256k tokens.

## Skills

| File | Skill | What it does |
|------|-------|--------------|
| [`humanizer.md`](humanizer.md) | **Humanizer** | Rewrites AI-sounding text (emails, report paragraphs, client notes) into plain business English. Facts, figures, names and domain terms stay exactly as written. |
| [`grill-me.md`](grill-me.md) | **Grill-Me** | Stress-tests a plan, draft or idea through rounds of questions, each with a recommended answer, then gives a summary before it writes anything. Starts only when you ask, for example with "grill me". |
| [`skill-creator.md`](skill-creator.md) | **Skill Creator** | Helps you create a new skill, improve an existing one, or turn a workflow or SOP into a skill. Tests the draft in chat before giving you a ready-to-upload file. |

### Full and lite versions

Humanizer and Grill-Me each come in a full version and a lite version. Lite versions are the earlier, shorter builds made for the old 10k character limit. They keep the core rules but drop most of the extra examples and edge-case handling.

| Skill | Full | Lite |
|-------|------|------|
| Humanizer | [`humanizer.md`](humanizer.md) (~20k chars) | [`humanizer-lite.md`](humanizer-lite.md) (~9k chars) |
| Grill-Me | [`grill-me.md`](grill-me.md) (~20k chars) | [`grill-me-lite.md`](grill-me-lite.md) (~10k chars) |
| Skill Creator | [`skill-creator.md`](skill-creator.md) (~38k chars) | — |

Install either the full or the lite version of a skill, not both, since they share the same triggers.

### Humanizer

- Finds and fixes 26 common AI writing patterns, such as "not X but Y" framing, forced lists of three, sales language, overused words ("delve", "robust") and chatbot phrases ("I hope this helps!").
- Each pattern has a **keep-condition**, so real caveats, technical terms, number ranges and named sources are left alone.
- Weak signals (dashes, stacked qualifiers, hyphenation, passive voice, curly quotes) are fixed only when several appear together.
- Matches a writing sample if you give one, and keeps shorthand in informal notes.
- Handles **follow-up requests** in the same chat ("shorter", "more formal", "keep paragraph 2 as it was").
- Includes a full worked example of a settlement fails email.
- Trigger it with: "humanize this", "make this sound less AI", "rewrite naturally".

### Grill-Me

- Asks up to 5 questions per round, most important first. A question that depends on another waits for a later round.
- Every question has a recommended answer, so replying "yes" accepts them all.
- Never asks for facts already in the pasted material. If you don't know a fact, it records an `ASSUMPTION` and moves on.
- A visible tracker shows what is settled and what is still open.
- Has **question banks** for five task types: emails, rough ideas, process flows, guides and presentations.
- Ends with a summary of decisions, glossary, assumptions and risks. It writes the deliverable only after you confirm.
- Type "summary" or "stop" at any time to jump to the summary.
- Includes worked examples for each task type, including one full session from start to finished email.
- Trigger it with: "grill me", "grill this", "stress-test this".

### Skill Creator

- Works in three modes, chosen automatically: **create** a new skill, **improve** a pasted skill, or **continue** a draft from earlier in the chat.
- For a new skill, asks up to 5 questions about the goal, triggers, a sample input, the output you want and any rules. It skips questions you've already answered.
- Tests the draft in chat with 2–3 made-up prompts, including a tricky one, and shows the results in a pass/fail table. Say "skip testing" to get the file straight away.
- Follows clear writing rules for the skills it makes: explain why a rule exists, use exact output templates, include worked examples, and keep everything in one file.
- Writes trigger descriptions that make the skill activate reliably, and gives 10 sample queries (5 that should trigger it, 5 that shouldn't) so you can check.
- Explains technical terms in plain words the first time it uses them.
- Trigger it with: "make a skill", "turn this into a skill", "improve this skill".

## Usage

Each file is a standard skill definition: YAML front matter (`name`, `description`) followed by the instructions.

To install one as a skill, create a folder named after the skill and save the file inside it as `SKILL.md`:

```
skills/
├── humanizer/
│   └── SKILL.md   ← humanizer.md (or humanizer-lite.md)
├── grill-me/
│   └── SKILL.md   ← grill-me.md (or grill-me-lite.md)
└── skill-creator/
    └── SKILL.md   ← skill-creator.md
```

The files are renamed in this repo only so they can sit side by side.

## Feedback

Suggestions and examples where a skill went wrong are welcome. Please open an issue.
