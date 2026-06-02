---
name: oracle
description: Use when the user types /oracle (or asks the oracle / answer machine / 答案机 / 答案之书 for an answer of the day, a fortune, or a yes-no / "should I…" wish). Draws one random playful, encouraging line from a pre-written bilingual pool (Chinese / English), independent of the question asked.
---

# oracle 🔮 — Answer Oracle

Draw one random line as the user's "answer of the day." This is a **fortune cookie**:
the answer is picked at random from a pre-written pool and is **independent of whatever
the user asked**. The question (if any) is ritual only — never read it to craft the answer.

## Inputs

The user may invoke it bare (`/oracle`), with a forced language (`/oracle zh`,
`/oracle en`), or with a question (`/oracle should I quit?`, `/oracle 我该辞职吗`).

## Step 1 — Pick the bank (language)

Walk this ladder, **stop at the first match**:

1. **Forced:** the argument is exactly `zh` or `en` → use that bank (don't treat it as a question).
2. **Question language:** there is a question argument → if it contains any Chinese
   (CJK) character, use `zh`; otherwise `en`.
3. **Conversation language:** bare `/oracle` → use the language this conversation has
   been happening in.
4. **System fallback:** still unsure (brand-new conversation, first thing typed) → run
   `case "$LANG" in zh*) echo zh ;; *) echo en ;; esac`.

The language only chooses the **bank**. It never chooses the line.

## Step 2 — Draw one line (truly random — do NOT pick yourself)

Run exactly one shell command. Set `LANG_CODE` to `zh` or `en` from Step 1:

```bash
d="$HOME/.claude/skills/oracle"; [ -f "$d/answers-en.txt" ] || d="$(pwd)"
shuf -n 1 "$d/answers-${LANG_CODE}.txt" 2>/dev/null \
  || awk -v seed="$RANDOM" 'BEGIN{srand(seed)} NF{a[++n]=$0} END{print a[int(rand()*n)+1]}' "$d/answers-${LANG_CODE}.txt"
```

`shuf` is preferred; the `awk` branch is the fallback when `shuf` is missing (common on
macOS). The `awk` branch is seeded with `$RANDOM` rather than the clock so that draws in
the same second don't repeat, and the `NF` guard skips blank lines. Use the command's
output verbatim — never substitute your own choice.

## Step 3 — Present it

Open with the crystal ball, then the drawn line as a blockquote. If the user gave a
question, echo it first for ritual (but the answer is unrelated to it).

English:
```
🔮 You give the crystal ball a shake…

> <the drawn line>
```

Chinese:
```
🔮 你摇了摇水晶球…

> <抽到的那句>
```

Keep it to the opener + the line. No explaining, no "this means…", no choosing a
"better" line. The whole charm is that it's random.
