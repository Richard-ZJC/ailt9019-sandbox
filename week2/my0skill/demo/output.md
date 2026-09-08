# my0skill · demo run on sample-proposal.md

> Invocation: check proposal against the 6-section template, then emoji-fy the headings and lead sentences without touching the "does not" boundary or technical terms.

---

## 1. Proposal check

| § | Section | Status | What is there | What's missing |
|---|---|---|---|---|
| 1 | Background | ✅ | user group (volunteer tutors, Y4–6), task (first-draft outline), pain (30–60 min wasted), motivation, **idea source stated (#5 adapted)** | — |
| 2 | Project pipeline | ✅ | end-to-end: input → parser → LLM → formatter → output; pipeline described | no figure yet (encouraged but not required) |
| 3 | Evaluation | ✅ | test style (8-scenario task set) + rubric + "good enough" bar + **refusal case (real names/rosters)** | — |
| 4 | Distribution | ⚠️ | GitHub Pages + README + portable skill file | missing the "model/API running without painful setup" story — say which model + how the user gets the key |
| 5 | Ethics & risks | ✅ | explicit **"does not"** (assess students / store rosters / claim accredited) + misuse + mitigation (footer + feedback link) | — |
| 6 | Checkpoints | ✅ | must/should split + timeline to W10 | (A) "must-have small enough to finish" — 3 items is fine ✅ |

### 5 self-check items
- ☐ one user group + one task in one sentence — ✅ "volunteer tutor → first-draft lesson outline"
- ☐ dataset source + permission in one line — ⚠️ source named ("short list of past lesson briefs the team writes by hand") but **permission line is missing** — add: "self-curated, internal use only"
- ☐ end-to-end runnable this semester — ✅
- ☐ at least one "does not" boundary — ✅
- ☐ idea source stated, no personal info — ✅

### Top 3 things to fix
1. **§4**: add the "how the tutor gets the model running" line (which model + where the API key lives).
2. **5-item check**: append the permission line for the self-curated dataset.
3. (Optional) **§2**: a one-box pipeline figure is encouraged, takes 5 min, looks better in the form.

---

## 2. Emoji-fied version

> Rule applied: max 1 emoji per short sentence, "does not" boundary left literal, technical terms (`LLM`, `rubric`, `GitHub Pages`, `skill file`) untouched.

---

## 1. Background 🍎
Tutors in after-school centres for Years 4–6 (ages 9-11) need a first-draft lesson outline when they only have a one-line topic brief 🚨
**tl;dr**: they spend 30–60 min re-formatting past lessons instead of teaching 🪨
The product is for **volunteer tutors** who teach small classes; it is **not** for accredited curriculum designers or schools that already use a paid LMS ❌
📝 Idea source: idea-list **#5**, adapted to a volunteer (non-formal) context.

## 2. Project pipeline ⚙️
User enters a one-line topic (e.g. "photosynthesis for Year 5") + class length ➡️
The agent returns a 3-section outline (warm-up / main / exit ticket) with timing and 2 example questions 🍰
**Pipeline**: input → topic parser → outline generator (LLM, with our prompt) → formatter (skill file) → output.
📝 Data: short list of past lesson briefs the team writes by hand; no real student data.

## 3. Evaluation and test ✅
Test style: 8 task-set scenarios (mix of subjects and class lengths) graded against a rubric written by a Year-5 teacher on the team 🧠
"Good enough" = rubric score ≥ 4/5 on 6/8 cases 🎯
🚨 Refusal case: the system must refuse any request that names a real student or a real class roster.

## 4. Distribution 🌐
Single-page web app on GitHub Pages. README walks a non-coder tutor through: open link → type topic → read outline 🍰
The outline skill is a standalone file that any agent with skill support can load 🛠️

## 5. Ethics ⚠️
**Does not**: assess real students, store class rosters, or claim the draft is an accredited curriculum.
**Misuse risk**: a school uses the draft as the official lesson plan → mitigated by an "unofficial draft, please review" footer and a one-click feedback link to the team 🛡️

## 6. Checkpoints 🗓️
**Must-have**: outline generator + refusal on real names + rubric grading script.
**Should-have**: history page, multi-language topic input.
🛤️ Timeline: W3 outline + refusal done → W4 draft (**Sep 25**) → W6 rubric grading → W10 demo.

---

> Notes for the reader:
> - The "does not" sentence is intentionally **not** emoji-fied — the SKILL.md contract says boundary language stays literal.
> - All section numbers and code-like strings (`Years 4-6`, `W3`, `#5`, `4/5`) stayed as-is.
> - Each section got **one header emoji** + a few sentence-level ones; not a free-for-all.
