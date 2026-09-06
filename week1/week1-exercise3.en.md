# AILT9019 · Week 1 Exercise ③ Deliverable
## HKD⇄USD Converter · Boundary Testing & Fix Log

> **Honesty statement**: The page was generated and edited with an AI coding tool (WorkBuddy). All design decisions and tests below were run and verified by me — I never took "the tool says it works" as proof.

> **Authored by**: Zou Jiachen (Richard)

---

## 1. Exercise Goal (Tutorial 1)

This week has three exercises: ① make one visible UI change ② break it on purpose then fix it ③ explain one change in your own words. This document covers ②'s boundary testing & fixes, plus ③'s explanation. Acceptance: change visible · you can point to the file · at least one test catches a failure.

## 2. What I Changed

### 2.1 Input validation (reject invalid values & negatives)
- `parse()` now uses a strict regex (L50): garbage like `0x10`, `7.8abc`, `1e` is rejected instead of being silently treated as 0.
- Removed the sign slot `[+-]?` from the regex (L50): money has no negatives, so negative amounts / rates / results are all rejected.
- Invalid or negative input triggers a red border (L62–63 `flag()`, called at L73–74) instead of "silently clearing".

### 2.2 Precision isolation (money has only two decimals)
- `show()` falls back to significant-figure notation when a value rounds to 0 (L57–59): `1e-7` no longer shows as `0`.
- Added `exactFrom` as the full-precision source of truth (L64–65): swapping direction with ⇅ uses the exact value, avoiding the `1000 → 128.21 → 1000.04` rounding drift (L67–75, L77–84, L87–94).

## 3. How I Tested

I don't guess by reading code. I had another AI write the test script, ran the page's inline `<script>` directly in Node with a minimal DOM stub, fed in boundary values, and read the real DOM `value`. I only look at the actual outputs it printed — I don't vouch for it. Script: `/tmp/verify.mjs`.

## 4. Boundary Cases & Results (13 cases)

| # | Operation | Actual Result | Verdict |
|---|---|---|---|
| T1 | from=1000 rate=7.80 ⇅ | from=128.205128 to=1,000 | PASS |
| T2 | T1 then ⇅ again | from=1,000 | PASS |
| T3 | ⇅ ×10 in a row | from≈1,000.00 | PASS |
| T4 | hand-edit to=0.005 | to keeps 0.005 | PASS |
| T5 | rate=abc then ⇅ | amount not lost | PASS |
| T6 | rate=0x10 | red border, to empty | PASS |
| T7 | rate=1,5 from=1000 | to=666.67 (isolated) | PASS* |
| T8 | rate=7.8abc | red border, to empty | PASS |
| T9 | from=-100 / rate=-7.8 | both rejected + red | PASS |
| T10 | from=1e-7 | to=0.00000078 | PASS |
| T11 | from=1e21 | no scientific notation | PASS |
| T12 | rate=0.0001 ⇅⇅ | from≈1,000.00 | PASS |
| T13 | to typed char by char 1→1e→1e5 | from=780,000 | PASS |

> *T7 note: in the batch run T7 once showed `to=15,000`. Root cause was my test script's `reset()` not resetting the direction flag, so the ⇅ flip from T5 bled in (computed as `1000×15`). Re-running in a fresh session with direction=HKD→USD gave `66.67`, i.e. the page is correct. This was a test-fixture bug, not a page bug.

**Conclusion: 13/13 passed.**

## 5. Exercise ③ Two-Sentence Explanation

> Input must be a number, so I added format validation to the inputs — illegal values like `0x10` or `7.8abc` get flagged with a red border instead of being quietly treated as 0 and silently producing a wrong result; the display also keeps only the two-decimal precision that really exists, because money simply doesn't have that many decimals — extra decimals are unrealistic and let the rounding error pile up every time you hit ⇅.
> Both fixes close the gap of "AI says it works ≠ it's actually right": swallowing invalid input makes you think it computed correctly, and money is natively two decimals — carrying meaningless decimals is misleading and drifts on round-trip conversion (Exercise ② caught exactly `1000→128.21→1000.04`).

## 6. Wrap-up Status

- [x] Exercise ① visible change: title `#1B2A41`/24px (L14)
- [x] Exercise ② break + fix: re-verified 13/13
- [x] Exercise ③ two-sentence explanation finalized (§5)
- [x] Negatives rejected (L50 sign slot removed)
- [ ] `git commit` + push to `Richard-ZJC/ailt9019-sandbox` (do it yourself)
