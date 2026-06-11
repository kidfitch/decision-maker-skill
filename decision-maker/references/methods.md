# Methods — Technical Reference

This file holds the formulas and rationale of the decision-science methods the decision-maker skill rests on. While running the SKILL.md flow, consult this whenever a calculation or rationale is needed.

## Table of Contents
1. ROC (Rank Order Centroid) — deriving weights from a ranking
2. Weighted scoring (MAUT / Weighted Scoring)
3. Sensitivity analysis (margin test + flip test)
4. Dual-process theory (Kahneman) + naturalistic decision making (Klein)
5. WRAP process (Heath & Heath) — the bias shield
6. Pre-mortem (Klein)
7. Type 1 / Type 2 decisions (triage logic)

---

## 1. ROC (Rank Order Centroid)

**Purpose:** Eliciting exact weights from a user (e.g., "cost is 35% important") is hard and unreliable. Instead the user only puts criteria in **order of importance**; ROC mathematically derives weights from that ranking. It is the best-known of the "surrogate weight" methods in the literature, and has held up robustly across many decision distributions in empirical tests.

**Formula:** For n criteria, the weight of the criterion in position i:

    w_i = (1/n) × Σ (1/k)   ,  for k = i to n

That is, starting from the bottom, sum the 1/k terms.

**Lookup table (common n values):**

| Rank | n=2 | n=3 | n=4 | n=5 | n=6 |
|------|-----|-----|-----|-----|-----|
| 1st  | .750| .611| .521| .457| .408|
| 2nd  | .250| .278| .271| .257| .242|
| 3rd  |     | .111| .146| .157| .158|
| 4th  |     |     | .063| .090| .103|
| 5th  |     |     |     | .040| .061|
| 6th  |     |     |     |     | .028|

(To convert to percent, ×100. Small differences from rounding are possible; the total should be ~1.00.)

**Worked example (n=4):**
- 4th rank: (1/4)(1/4) = 0.0625
- 3rd rank: (1/4)(1/4 + 1/3) = (1/4)(0.5833) = 0.1458
- 2nd rank: (1/4)(1/4 + 1/3 + 1/2) = (1/4)(1.0833) = 0.2708
- 1st rank: (1/4)(1/4 + 1/3 + 1/2 + 1/1) = (1/4)(2.0833) = 0.5208

**Known weakness:** ROC is biased toward the top-ranked criterion (it assigns a relatively high weight to the 1st criterion). So if the 1st criterion alone exceeds 50%, warn the user — the decision may have collapsed onto a single criterion. If desired, a more balanced alternative is Rank Sum (RS): w_i = (n − rank_i + 1) / Σ(all ranks). RS is less biased toward the top criterion but has lower discriminating power. The default is ROC.

---

## 2. Weighted Scoring (MAUT / Weighted Scoring)

Total score for each option:

    Score(option) = Σ (w_i × p_i)

- w_i = the ROC weight of criterion i (sums to 1)
- p_i = the option's score on that criterion (1–10)

Higher score = better overall fit. This is a "compensatory" method: a low score on one criterion can be offset by a high score on another. If a criterion is a "must-have" (non-compensatory), ask the user and treat it separately as a threshold/veto — don't bury it in the matrix.

---

## 3. Sensitivity Analysis

How "robust" is the matrix result? Two tests:

**Margin test:** If the score gap between the top two options is less than 10% of the higher score → "statistical tie." This means the matrix is saying "these two are even"; the decision is hidden in a missing criterion or in the weight preference.

**Flip test (rank reversal):** Swap the order of the top two criteria (hence swap their weights), recompute the matrix. If the winner changes, the decision critically depends on the relative importance of those two criteria. Present it to the user as a single reduced question: "Is X more important than Y?" — the answer to that is the decision itself.

---

## 4. Dual-Process Theory + Naturalistic Decision Making

**Kahneman (System 1 / System 2):** System 1 is fast, intuitive, automatic, emotionally charged; System 2 is slow, analytical, effortful. The classic (mistaken) reading says "trust System 2, distrust System 1."

**Klein (naturalistic / RPD):** Experts in familiar domains don't compare options; they recognize a pattern and take the first workable option, which is usually right. So intuition, in a familiar domain, is **compressed experience**, not noise.

**Synthesis (the skill's stance):** "Which system is right" is the wrong question. The right one: in this task, at this person's experience level, which mode has the better error profile? Familiar domain + experienced person → weight intuition as data. Unfamiliar domain + novice → lead with analysis. This is why Stage 1 asks for expertise calibration and the end runs an intuition–matrix confrontation: a conflict is the signature of a criterion the matrix missed.

---

## 5. WRAP Process (Heath & Heath, "Decisive")

Four shields against the four "villains" of decision-making. Embedded across the skill's stages:

- **W — Widen options:** Against narrow framing. "Are these really the only two options?" → Stage 1.
- **R — Reality-test assumptions:** Against confirmation bias. "If you were wrong, which score would be off?" → Stage 3.
- **A — Attain distance:** Against short-term emotion. Regret minimization + "what would you tell a friend?" → Stage 4 closing.
- **P — Prepare to be wrong:** Against overconfidence. Pre-mortem → Stage 4.

Note: the "vanishing options" technique (a tool of W) — "if none of these options were available, what would you do?" — can be used to generate new options when the user is stuck between two.

---

## 6. Pre-Mortem (Klein)

Prospective hindsight: "It's a year from now, this choice failed spectacularly. Why?" List every failure cause. It surfaces the risks optimism and overconfidence hide, because "why did it fail" is psychologically more productive than "what are the risks." In the skill it runs AFTER the matrix winner is known but BEFORE the final recommendation; if a serious risk emerges, you can return to the matrix.

---

## 7. Type 1 / Type 2 Decisions (Triage)

Bezos's door metaphor:
- **Type 2 (two-way door):** Reversible decision. If wrong, you walk back. Should be made fast — over-analysis is waste. → FAST MODE.
- **Type 1 (one-way door):** Irreversible / hard-to-undo decision. Should be made slowly, carefully, with full analysis. → DEEP MODE.

The two triage questions (reversible? + large impact?) operationalize this distinction. The goal: don't tire the user on a small decision, don't stay shallow on a big one.
