# My Skills

A collection of custom Claude skills (prompt instruction files) for writing and planning work.

## Skills

| File | Skill | What it does |
|------|-------|--------------|
| [`humanizer.md`](humanizer.md) | **Humanizer** | Rewrites AI-sounding text (emails, report paragraphs, client notes) into clear, natural business English without changing any facts, figures, names or meaning. Targets 26 common AI writing patterns, such as "not X but Y" framing, forced triads, sales language and chatbot residue. |
| [`grill-me.md`](grill-me.md) | **Grill-Me** | Stress-tests a plan, draft or idea through structured rounds of questions (max 5 per round). Each question comes with a recommended answer, tracks settled vs. open decisions, and ends with a copy-pasteable summary before any deliverable is written. Activates only on explicit triggers like "grill me". |

## Usage

Each file is a standard skill definition with YAML front matter (`name`, `description`) followed by the instructions.

To install one as a Claude skill, create a folder named after the skill and save the file inside it as `SKILL.md`:

```
skills/
├── humanizer/
│   └── SKILL.md   ← humanizer.md
└── grill-me/
    └── SKILL.md   ← grill-me.md
```

The files were renamed in this repo only so the two can sit side by side.
