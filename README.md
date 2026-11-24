# My Skills

A collection of custom skills for writing and planning work in securities services.

> **Model:** Tuned for **Gemma 4 26B MoE**. Results on other models may differ.
> **Size:** Each skill is about 20k characters, under the 40k character skill limit. The model's context window is 256k tokens.

## Skills

| File | Skill | What it does |
|------|-------|--------------|
| [`humanizer.md`](humanizer.md) | **Humanizer** | Rewrites AI-sounding text (emails, report paragraphs, client notes) into plain business English. Facts, figures, names and domain terms stay exactly as written. |
| [`grill-me.md`](grill-me.md) | **Grill-Me** | Stress-tests a plan, draft or idea through rounds of questions, each with a recommended answer, then gives a summary before it writes anything. Starts only when you ask, for example with "grill me". |

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

## Usage

Each file is a standard skill definition: YAML front matter (`name`, `description`) followed by the instructions.

To install one as a skill, create a folder named after the skill and save the file inside it as `SKILL.md`:

```
skills/
├── humanizer/
│   └── SKILL.md   ← humanizer.md
└── grill-me/
    └── SKILL.md   ← grill-me.md
```

The files are renamed in this repo only so the two can sit side by side.

## Feedback

Suggestions and examples where a skill went wrong are welcome. Please open an issue.
