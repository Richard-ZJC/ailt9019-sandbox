---
name: my0skill
description: Use this skill when reviewing AILT9019 proposal drafts against the 6-section template, and when turning plain sentences into emoji-spiced, casually funny copy without changing meaning. Two jobs: (1) check the proposal hits all 6 sections + 5 self-check items, (2) sprinkle emojis and swap a few flat words for fun, conversational ones.
---

# my0skill · Proposal Checker + Emoji Sprinkler

## When the assistant should reach for this skill
- The user pastes or writes a **proposal draft** for AILT9019 (or any 6-section template using the same shape).
- The user asks to **make a sentence funnier / more casual / with emojis** while keeping the meaning.
- Both at once: the user wants a draft that is **template-complete AND emoji-fied**.

## What the skill produces (in one pass)

### Job 1 · Proposal check
Compare the input against the template:

| § | Section | Must contain |
|---|---|---|
| 1 | Background | one user group + one task + current pain + motivation + **idea source** |
| 2 | Project pipeline | end-to-end path (problem → method → output); pipeline figure encouraged |
| 3 | Evaluation and test | test style (task set / walkthrough / comparison / refusal) + at least **one safe-fail/refusal case** |
| 4 | Distribution & accessibility | model/API setup story · README-or-UI hand-off · multi-agent portability |
| 5 | Ethics & risks | explicit **"does not"** · top misuse/failure modes · mitigations |
| 6 | Checkpoints | (A) must-have vs should-have · (B) timeline to W4 draft and beyond |

5 self-check items to flag:
- ☐ one user group + one task in **one sentence**
- ☐ dataset source + permission in **one line**
- ☐ end-to-end runnable this semester
- ☐ at least one **"does not"** boundary
- ☐ Background states idea source, and **no personal info** (names / UIDs / emails)

Output format: a table per section with `Status ✅/⚠️/❌ · what is there · what's missing`, then a "top 3 things to fix" list.

### Job 2 · Emoji + casual rewrite
Sprinkle emojis + swap a few flat words for fun ones, without changing meaning.

**Rules** (don't break these):
1. **Substitute only when the meaning is unmistakable**: 🦊 for fox, ✅ for done, 🚨 for urgent, 💡 for tip/idea, ⚠️ for warning, 🛠️ for fix, 🧠 for think/idea, 📝 for note, 🍰 for easy, 🪨 for hard, ⚡ for fast, ➡️ for next, ⬅️ for previous, ❓ for question, 💬 for answer.
2. **Punctuation emojis** go at the **end** of a sentence, replacing a period or exclamation.
3. **Bookend** the message with the same emoji when the message is a single short line (e.g. `✨ Big news dropping tomorrow! ✨`).
4. **List items**: each bullet starts with a topical emoji (🍝 🍕 🏛️ for Rome tips), not a generic ✨.
5. **Never replace a number, a code, a file path, a proper noun, or a quoted error message.** Those stay literal.
6. **Never replace a word that is part of the user's specific phrasing** (e.g. their stated "does not" boundary) — emoji-fy the framing around it, not the boundary itself.
7. **Don't over-decorate**: max 1 emoji per short sentence, 2-3 per paragraph. If it reads like a 2010 chain email, dial it back.
8. **Keep technical terms literal**: `regex`, `MCP`, `parse`, `branch`, `commit` — no emoji, no replacement.

**Casual word swaps** (English, light touch only — don't slang-ify academic text):
| Plain | Casual+emoji |
|---|---|
| very important | 🚨 critical / heads-up |
| in short / to summarize | tl;dr: |
| note that | 📝 heads-up: |
| for example | 🧪 e.g. |
| remember | 🧠 keep in mind |
| done | ✅ done |
| not done | ❌ nope |
| warning | ⚠️ watch out |
| quick win | 🍰 easy win |
| actually | tbh |
| I think | 🧠 IMO |
| basically | esp. (or just delete) |

## Output contract
When invoked, reply in this exact shape (the user can copy-paste into a doc):

```
## 1. Proposal check
[per-section table · 5 self-check bullets · top-3 fix list]

## 2. Emoji-fied version
[rewritten text, sentences still readable, technical terms preserved]
```

If only one job is requested, do only that one and label the section clearly.

## Limits (what this skill does NOT do)
- Does not invent dataset sources. If the draft doesn't name one, flag it; don't make one up.
- Does not remove the "does not" boundary. Only adds framing emojis around it.
- Does not translate the document. If the input is mixed Chinese/English, leave each language as-is — emoji can sit on either side.
- Does not push to GitHub. That's a separate step the user does.
