# oracle 🔮 — Answer Oracle Skill Design

Date: 2026-06-01
Status: to be implemented

## In one line

A fortune-cookie-style Claude Skill: `/oracle` draws a random **playful, encouraging** line as your "answer of the day", auto-detects Chinese/English, and is **independent of whatever you ask**.

> oracle = an oracle 🔮, and also a pun on the Oracle database.

## Goals

- A lightweight, fun, grab-and-go "make a wish / get an answer" toy.
- Fortune-cookie style: answers are a pre-written pool; draw one at random.
- One bilingual command: a **single `/oracle`** with separate Chinese and English answer banks, picking the bank automatically at runtime.
- Truly random: run one line of shell to draw, rather than letting Claude pick (avoids bias and repeats).

## Non-goals

- **No brand ties**, no food / commercial elements. "oracle" means only "an oracle" — it does not trade on the Oracle database trademark.
- **No reading the question to answer it**: the question is just ritual. Language detection only chooses **which bank**, never **which line**.
- No MCP / server / network / persistent stats. Pure Skill.

## Form

A single personal skill installed under `~/.claude/skills/`, usable in any project:

```
~/.claude/skills/oracle/
  SKILL.md
  answers-zh.txt   (Chinese, 500 lines)
  answers-en.txt   (English, 500 lines)
```

Source files are version-controlled in this repo; sync to the skills directory after install.

## Command

- Main command: **`/oracle`** (pure ASCII).
- Optional alias `/答案`: depends on whether Claude Code supports non-ASCII slash commands — add it if so, otherwise skip.

## Language detection

At runtime `/oracle` picks a bank by a priority ladder, **stopping at the first match**:

1. **Forced switch**: argument is exactly `zh` / `en` → use that language.
2. **Question language**: a question argument is present → detect the question's language (contains Chinese characters → Chinese bank, otherwise English bank).
3. **Conversation language**: bare `/oracle` → follow the current conversation's language. (Runtime judgment, not unit-tested.)
4. **System fallback**: still ambiguous → check `$LANG`: `case "$LANG" in zh*) Chinese ;; *) English ;; esac`

Language detection **only selects the bank**; once selected, draw one line at random with `shuf` as usual.

## Draw mechanism

SKILL.md instructs Claude to run one line of shell to draw, rather than picking itself:

- Preferred: `shuf -n 1 answers-<lang>.txt`
- Fallback (no `shuf` — common on macOS): `awk -v seed="$RANDOM" 'BEGIN{srand(seed)} NF{a[++n]=$0} END{print a[int(rand()*n)+1]}' answers-<lang>.txt`. Seed with `$RANDOM`, not the clock — bare `srand()` seeds from time-in-seconds, so rapid draws in the same second repeat. The `NF` guard skips blank lines so a trailing newline can't draw an empty answer.

## Content

- Tone: **playful + encouraging**, gentle and healing, with a touch of mystical open-endedness. ✨ as an accent.
- Count: **500 unique lines** each for Chinese and English (dedup the pool so repeated draws don't repeat).
- Length: short enough to read at a glance — Chinese **≤ 24 characters** / English **≤ 60 characters**.
- Organized by **series** (variety guaranteed when writing; the full pool is random when drawing): courage, comfort, childlike wonder, timing, play. The English bank mirrors the same series but is **original**, not a translation.

Examples:

- 「今天不用长大,你已经很勇敢了。」
- 「再等等,好东西正在路上,它只是迷路了一下下。」
- "Yes. Go do the brave little thing."
- "The answer is closer than you think. Turn around."

## Output

A 🔮 opener **in the chosen language**, then the answer on its own line as a blockquote. No fixed-width box — that avoids display-width math over CJK / emoji (easy to get wrong) and lets answers wrap naturally. An optional question argument is echoed for ritual only and **does not affect the draw**:

```
🔮 You give the crystal ball a shake…

> It's okay not to grow up today — you're already brave.
```

```
🔮 你摇了摇水晶球…

> 今天不用长大,你已经很勇敢了。
```

## Success criteria

- `/oracle` in any directory draws one answer with a 🔮 opener.
- All four language-detection rungs work; repeated draws are visibly non-repetitive.
- Each bank has 500 unique lines, no brand / food words, consistent tone.
- `/oracle <question>` echoes the question but the answer is unrelated to it.

## Testing

- Run the draw N times; confirm no errors, no repeat bias, clean rendering (opener + blockquote).
- Verify all four language rungs: Chinese question / English question / `zh`|`en` forced / bare command falling back under `LANG=zh_CN` and `en_US`.
- Verify the fallback branch in an environment without `shuf`.
- Spot-read ~30 lines (half Chinese, half English); confirm no brand / food words, no off-topic lines, glanceable length.
