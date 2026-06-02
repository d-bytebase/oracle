# oracle 🔮

A fortune-cookie-style [Claude Code](https://claude.com/claude-code) skill. Type
`/oracle` and it draws a random, playful, encouraging **answer of the day** from a
pre-written pool — bilingual (Chinese / English), auto-detected, and independent of
whatever you ask.

> oracle = an oracle 🔮, and a pun on the Oracle database.

## Installation

`oracle` is a **personal skill** — install it once into `~/.claude/skills/` and it
works in every project. No build step, no dependencies.

### 1. Get the files

```bash
git clone https://github.com/d-bytebase/oracle.git
cd oracle
```

(Or, if you already have the repo, just `cd` into it.)

### 2. Copy the skill into place

```bash
mkdir -p ~/.claude/skills/oracle
cp SKILL.md answers-zh.txt answers-en.txt ~/.claude/skills/oracle/
```

That's it. The skill needs only those three files: `SKILL.md` and the two answer
banks. `shuf` is used if present and falls back to `awk` automatically, so there's
nothing else to install (macOS works out of the box).

### 3. Verify

Start (or restart) Claude Code in any directory and run:

```
/oracle
```

You should get a 🔮 card with a random line. If `/oracle` isn't recognised, make sure
the files landed at `~/.claude/skills/oracle/SKILL.md` and restart Claude Code so it
picks up the new skill.

### Updating

Pull the latest and re-copy:

```bash
cd oracle && git pull
cp SKILL.md answers-zh.txt answers-en.txt ~/.claude/skills/oracle/
```

### Uninstall

```bash
rm -rf ~/.claude/skills/oracle
```

## Usage

```
/oracle                 # answer in the conversation's language
/oracle zh              # force Chinese
/oracle en              # force English
/oracle should I ship?  # ask a question (ritual only — the answer is unrelated)
```

## How it works

- Answers are pre-written (**500 zh + 500 en**) and drawn with `shuf` (or an `awk`
  fallback) — truly random. Claude never "picks" the line, so there's no bias or favourites.
- Language detection only chooses **which bank**; the draw itself is always random.
- The question is **ritual only** — the answer has nothing to do with it.

See the [design spec](docs/superpowers/specs/2026-06-01-answer-oracle-skill-design.md).
