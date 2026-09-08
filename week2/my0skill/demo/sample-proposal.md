# Mini proposal draft (for my0skill demo · v0)
> Group of 3 · AILT9019 · idea adapted from idea #5 (Lesson-plan scaffolder)

## 1. Background
Tutors in after-school centres for Years 4–6 (ages 9–11) need a first-draft lesson outline when they only have a one-line topic brief. Current pain: they spend 30–60 min re-formatting past lessons instead of teaching. The product is for volunteer tutors who teach small classes; it is not for accredited curriculum designers or schools that already use a paid LMS. Idea source: idea-list #5, adapted to volunteer (non-formal) context.

## 2. Project pipeline
User enters a one-line topic (e.g. "photosynthesis for Year 5") + class length. The agent returns a 3-section outline (warm-up / main / exit ticket) with timing and 2 example questions. Pipeline: input → topic parser → outline generator (LLM, with our prompt) → formatter (skill file) → output. Data: short list of past lesson briefs the team writes by hand; no real student data.

## 3. Evaluation and test
Test style: 8 task-set scenarios (mix of subjects and class lengths) graded against a rubric written by a Year-5 teacher on the team. "Good enough" = rubric score ≥ 4/5 on 6/8 cases. Refusal case: the system must refuse any request that names a real student or a real class roster.

## 4. Distribution
Single-page web app on GitHub Pages. README walks a non-coder tutor through: open link → type topic → read outline. The outline skill is a standalone file that any agent with skill support can load.

## 5. Ethics
Does not: assess real students, store class rosters, or claim the draft is an accredited curriculum. Misuse risk: a school uses the draft as the official lesson plan → mitigated by an "unofficial draft, please review" footer and a one-click feedback to the team.

## 6. Checkpoints
Must-have: outline generator + refusal on real names + rubric grading script. Should-have: history page, multi-language topic input. Timeline: W3 outline + refusal done; W4 draft (Sep 25); W6 rubric grading; W10 demo.

---

# Quick self-check (5 items)
- one user group + one task
- dataset source + permission
- end-to-end runnable
- one "does not" boundary
- idea source stated, no personal info
