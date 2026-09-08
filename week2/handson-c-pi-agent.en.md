# Hands-on C: Run Pi Agent End to End

**Course:** AILT9019 · AI Literacy II · Week 2 · Tutorial 2
**Name:** Zou Jiachen (Richard)
**Date:** 2026-09-08
**Agent:** Pi Agent (`@earendil-works/pi-coding-agent` v0.85.1)
**Model:** `qwen3:4b` (local, via ollama)

---

## 1. What I set up

- **Pi Agent** installed globally (`npm i -g @earendil-works/pi-coding-agent`), runs as a TUI.
- **Local model provider:** instead of an OpenAI/Anthropic API key (blocked by my network), I pointed
  Pi at a local **ollama** server (`http://localhost:11434/v1`, OpenAI-compatible endpoint) and pulled
  a lightweight model `qwen3:4b` (2.5 GB) for testing. Livable on my machine, no paid key needed.
- **My skill (Hands-on A) added:** copied `my0skill` (proposal checker + emoji sprinkler) into
  `~/.pi/agent/skills/my0skill/`. Skill discovery requires a valid YAML frontmatter — I had to fix
  my own SKILL.md (a description containing `: ` broke the `yaml` v2.9.0 parser) before Pi would
  load it.
- **MCP server (Hands-on B) added:** `fetch` (`mcp-server-fetch` via `uvx`) written to
  `~/.pi/agent/mcp.json`, with the MCP client capability provided by the community package
  `pi-mcp-adapter` (v2.32.1, `pi install npm:pi-mcp-adapter`).

## 2. The task I ran

I asked Pi to apply **my0skill** to a plain paragraph and turn it into a casual, emoji-sprinkled
version while keeping the meaning.

Pi's actual output (verbatim):

```
## 2. Emoji-fied version

AI4Math just leveled up! 🦊 Math teachers are now chatbots solving equations with emoji
logic, and calculus class became a TikTok dance party where students swing with numbers
🔢 and symbols 💡. 🚨 No degree needed—just vibes, zero panic, and 100% fun! 😄✨
```

## 3. Did Pi follow my skill?

**Yes — the output matches my0skill's rules.** My skill says:

- use emojis as sentence-ending punctuation (🦊😄✨ placed at ends),
- keep technical terms literal (»AI4Math«, »calculus«, »equations« untouched),
- swap a few flat words for casual ones (»just leveled up«, »just vibes«, »zero panic«),
- cap emoji density so it stays readable.

The result follows all of those. So the acceptance line **"Pi Agent follows your skill"** is met.

## 4. One thing I must be honest about: MCP was *not* actually called

Pi itself told me:

> "Since the mcp tool isn't found (probably a registration issue), I simulated the output
> based on the skill's rules."

So the emoji output above was **simulated**, not produced by a real `fetch`/tool call. The MCP
config and adapter are installed on my machine, but **in that Pi session the tool did not appear**,
so Pi never invoked it. This is exactly the trap the course warns about: *"never treat 'the AI said
it works' as evidence that it works."*

To actually verify MCP end to end I need to fully restart Pi, run `/mcp` (confirm/enable `fetch`),
then ask it to fetch a real URL and show me the output. I did confirm the tool works — but on a
different path: I tested `mcp-server-fetch` directly over stdio (see Hands-on B), where it returned
real pages from `example.com` and `workbuddy.cn`.

## 5. Acceptance check

| Requirement (from Tutorial 2) | Status |
|---|---|
| Install Pi Agent and start a session in a practice folder | ✅ v0.85.1 running |
| Run a task that creates something, then ask it to explain | ✅ emoji-style rewrite (my0skill) |
| Add your skill from Hands-on A and verify it is used | ✅ output follows my0skill rules |
| If you have a model provider, also connect the MCP server from B | ⚠️ config + adapter installed; **runtime tool call not yet verified in Pi** (Pi simulated) |
| Push the practice folder to GitHub | ⏳ pending (see note) |
| Acceptance: Pi Agent runs on your machine and follows your skill | ✅ runs + follows skill |

## 6. One honest takeaway

Getting a skill "used" and getting an MCP tool "called" are two different levels of proof. The skill
was genuinely followed — the output matches my rules. The MCP call is **claimed but unverified**; the
screenshot literally shows Pi admitting it simulated. I'd rather mark that gap than record a success
I haven't reproduced. This whole exercise is why the course wants us to verify, not to take the AI's
word for it.
