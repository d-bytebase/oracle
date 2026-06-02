# oracle 🔮

A fortune-cookie-style [Claude Code](https://claude.com/claude-code) skill. Type
`/oracle` and it draws a random, playful, encouraging **answer of the day** from a
pre-written pool — bilingual (Chinese / English), auto-detected, and independent of
whatever you ask.

> oracle = an oracle 🔮, and a pun on the Oracle database.

## Install (personal skill)

```bash
mkdir -p ~/.claude/skills/oracle
cp SKILL.md answers-zh.txt answers-en.txt ~/.claude/skills/oracle/
```

Then, in any project:

```
/oracle                 # answer in the conversation's language
/oracle zh              # force Chinese
/oracle en              # force English
/oracle should I ship?  # ask a question (ritual only — the answer is unrelated)
```

## How it works

- Answers are pre-written (**500 zh + 500 en**) and drawn with `shuf` — truly random.
  Claude never "picks" the line, so there's no bias or favourites.
- Language detection only chooses **which bank**; the draw itself is always random.
- The question is **ritual only** — the answer has nothing to do with it.

See the [design spec](docs/superpowers/specs/2026-06-01-answer-oracle-skill-design.md).
