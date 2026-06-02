# oracle 🔮

A fortune-cookie-style [Claude Code](https://claude.com/claude-code) skill. Ask the
oracle and it draws a random, playful, encouraging **answer of the day** from a
pre-written pool — bilingual (Chinese / English), auto-detected, and independent of
whatever you ask.

> oracle = an oracle 🔮, and a pun on the Oracle database.

## Installation

Install it from the Claude Code **plugin marketplace** — no file copying, no dependencies.

In Claude Code, add this repo as a marketplace, then install the plugin:

```
/plugin marketplace add d-bytebase/oracle
/plugin install oracle@oracle
```

The plugin bundles everything it needs (`SKILL.md` + the two answer banks). `shuf` is
used if present and falls back to `awk` automatically, so macOS works out of the box.
Use the `/plugin` menu to browse, update, or remove installed plugins.

## Usage

Plugin skills are namespaced `plugin:skill`, so the command is `/oracle:oracle`:

```
/oracle:oracle                 # answer in the conversation's language
/oracle:oracle zh              # force Chinese
/oracle:oracle en              # force English
/oracle:oracle should I ship?  # ask a question (ritual only — the answer is unrelated)
```

You can also just ask the oracle in plain language — Claude triggers the skill from its
description.

## How it works

- Answers are pre-written (**500 zh + 500 en**) and drawn with `shuf` (or an `awk`
  fallback) — truly random. Claude never "picks" the line, so there's no bias or favourites.
- Language detection only chooses **which bank**; the draw itself is always random.
- The question is **ritual only** — the answer has nothing to do with it.

See the [design spec](docs/superpowers/specs/2026-06-01-answer-oracle-skill-design.md).
