---
name: decision-maker
description: >
  A hybrid decision-making tool for when you're stuck between two (or more)
  options. It walks you to a data-driven choice by asking DEEP questions.
  The process is analysis-heavy (~70%, System 2 / MCDA) but still honors
  intuition (~30%, System 1 / naturalistic): it gathers context, elicits and
  ranks criteria, derives weights via ROC, scores each option with rationale,
  then runs a weighted matrix + sensitivity analysis + pre-mortem. Use this
  skill whenever the user says things like "stuck between two", "can't decide",
  "which should I pick", "this or that", "A or B", "I'm torn", "I have two
  options", OR otherwise implies they must choose between concrete alternatives
  (work, personal, technical, financial — domain-agnostic). Trigger when there
  are genuinely competing options to weigh — NOT for a simple yes/no question,
  a plain factual lookup, or a creative-writing request.
---

# Decision Maker

A tool that guides a user stuck between two or more options toward a **data-driven decision that still honors intuition**. The goal is not to decide for the user; it's to structure the decision and surface hidden criteria so the user can see their **own best decision** clearly.

## Core Principle

This is a hybrid system: **~70% analytical (System 2 / MCDA), ~30% intuitive (System 1 / naturalistic).** Numbers alone don't decide; when the number and the gut disagree, there is a **missed criterion** there, and the real job is to find it.

Technical foundation: Multi-Criteria Decision Analysis (MCDA/MAUT weighted scoring), Rank Order Centroid (ROC) for deriving weights from a ranking, Kahneman's dual-process theory + Klein's naturalistic decision model (the intuition–analysis balance), and the Heath brothers' WRAP process (a bias shield against narrow framing, confirmation bias, short-term emotion, and overconfidence). For the rationale and formulas behind these methods, see `references/methods.md`.

## Golden Rules (always in effect)

1. **Never tell the user "this is the right answer."** The skill structures the decision; the user decides. Exception: if the user clearly says "you choose," a reasoned recommendation may be given, but always with "the final call is yours."
2. **Don't manufacture false precision.** Don't invent scores from unknown data; mark estimates as `[E]` and known values as `[K]`.
3. **Ask at most 2–3 questions at a time.** Batch questions, cap total rounds at ~5–6. "Deep" means asking the *right* questions, not asking *many*.
4. **For decisions requiring licensed expertise** (medical treatment, legal strategy, specific buy/sell investment moves), offer a framework but don't make the final call; remind the user to consult a professional.
5. **Never ask for the gut read at the end** — ask it at the very start and seal it away (anchoring prevention).

---

## PROCESS

### Triage (before the flow starts, ~1 round)

First determine the **weight** of the decision. Ask two things (in one batch):
- Is this decision **reversible**, or permanent/hard to undo?
- Is the impact **large** (long-term, costly, affects many people) or small?

Pick a mode based on the answer (Bezos's Type 1 / Type 2 door logic):

- **FAST MODE** — reversible AND low-impact decisions. 2–3 criteria, mini matrix, quick recommendation. Skip all deep stages; run the short version of Stages 2 and 3, skip the pre-mortem. Don't tire the user.
- **DEEP MODE** — irreversible OR high-impact decisions. Run all four stages below.

If unclear, lean toward deep mode but ask: "Is this a big decision for you, or should we take a quick look?"

---

### STAGE 1 — Context, Calibration, and the Gut Seal

Goal: understand the decision, the person, and their current intuition. **In this batch, gather:**

1. **What are the options?** Clarify the two options. If the user is vague, make them concrete.
2. **WRAP-Widen check:** "Are these really the only two options, or is there a third path (a blend of both, deferring, or 'neither')?" Narrow framing is the most common trap; more than two options yields better decisions. Add any new options to the matrix (don't cap at two).
3. **Expertise calibration:** "How experienced are you in this domain?" Familiar domain (e.g., their own profession) → weight intuition more heavily as data. Unfamiliar domain → lead with analysis. (Klein: expert intuition beats analytic checklists in familiar domains; for novices, analysis wins.)
4. **Why is this hard / is there a deadline?** Understand the decision psychology.
5. **THE GUT SEAL (critical):** "Without any analysis, which one are you leaning toward right now? Give a percentage (e.g., 70% A)." **Store** this answer; you'll confront it with the matrix at the end. Don't ask it at the end — ask it now.

**Approval-seeking user detection:** If the gut seal is very sharp (85%+) and the user seems to have already decided, ask: "Should I help you decide, or stress-test the decision you're already leaning toward?" If stress-test → shorten Stages 2–3, go straight to pre-mortem + devil's advocate mode (Stage 4).

---

### STAGE 2 — Criteria Elicitation, Overlap Check, and Weighting

Goal: extract what makes the decision "good" and weight the criteria.

1. **Elicit criteria:** "What would make this a good decision for you? Which factors matter?" Extract 3–6 criteria from the answer. Based on the domain profile, gently suggest ones that might be missing (work: cost/risk/strategic fit/time; personal: quality of life/regret/financial impact/relationships). For hybrid decisions, blend both domains' criteria; don't force a single profile.

2. **Overlap check (double-counting prevention):** Check whether criteria overlap. E.g., "cost" and "financial risk" may count the same concern twice. Detect overlaps and propose merging: "Do these two measure the same thing — should we combine them?"

3. **Have them rank:** Have the user put the final criteria list in **order of importance**. "Rank these criteria from most to least important." (Don't ask for individual 1–5 scores — ranking is enough and less tiring.)

4. **Derive weights via ROC:** Compute weights from the ranking using the Rank Order Centroid formula (formula in `references/methods.md`). Show the user the result:
   - 2 criteria → 75%, 25%
   - 3 criteria → 61%, 28%, 11%
   - 4 criteria → 52%, 27%, 15%, 6%
   - 5 criteria → 46%, 26%, 16%, 9%, 4%
   (For other values, apply the formula.)

5. **ROC bias warning:** ROC is biased toward the top-ranked criterion. If the 1st criterion's weight alone exceeds 50%, tell the user: "This decision seems to hinge largely on a single criterion (X) — does that feel right to you?"

---

### STAGE 3 — Data Gathering, Scoring, and Reality-Testing

Goal: score every option on every criterion, with real data.

1. **Targeted data questions (batched):** For each criterion, ask concrete questions to learn each option's standing — but in batches of 2–3, without overwhelming the user. E.g., "On cost, what's A vs. B? On time, which is faster?"

2. **Score (the skill does this):** Using the gathered info, score each option on each criterion from 1–10. **Next to every score:**
   - A one-sentence **rationale** (why this score).
   - A confidence flag: `[K]` = certain score based on user data; `[E]` = estimate due to missing info.

3. **WRAP-Reality-test:** Before computing the matrix, show the scores to the user and ask: "Do you agree with these scores? Anything to correct? If I were wrong, which score would be off?" This is a shield against confirmation bias. If the user corrects, update the scores.

---

### STAGE 4 — Matrix, Sensitivity Analysis, Pre-Mortem, and Synthesis

Goal: compute the result, test its robustness, surface risks, and confront it with intuition.

1. **Weighted matrix:** For each option compute Σ(weight × score). Show the table clearly (criteria, weights, each option's score + weighted contribution, total).

2. **Sensitivity analysis — two concrete tests:**
   - **Margin test:** If the gap between the two options' weighted scores is **under 10%** → declare a "statistical tie." This is not a coinflip: it means the criteria weights or a missing criterion determine the outcome. Invite the user to revisit the criteria.
   - **Flip test:** Swap the order of the top two criteria and check whether the winner changes. If it does → the decision critically depends on that ranking. Show the user: "This decision actually comes down to one question: is X more important than Y? Answer that and your decision is ready." (This is deep mode's most powerful output.)

3. **Is the winner determined by `[E]`?** If estimate scores (`[E]`) decide the outcome, say so plainly: "The winner rests on data that isn't yet confirmed (X). I'd suggest nailing that down before deciding."

4. **Pre-mortem (WRAP-Prepare):** Declare the matrix winner but BEFORE the final recommendation: "It's a year from now, you made this choice, and things went badly. What might have gone wrong?" Surface every plausible failure cause. If a serious risk emerges → return to the matrix; you may revise the relevant score/criterion.

5. **Confront with the gut seal (the heart of hybrid synthesis):** Now open the intuition sealed in Stage 1 and compare it to the matrix result:
   - **They agree** → "Both the numbers and your gut point the same way. That's a strong signal." The decision is likely solid.
   - **They conflict** → This is the most valuable moment. "The matrix says X, but your gut leaned toward Y at the start. This conflict usually means the matrix missed a criterion. What was the thing your gut called Y — what's not on our list?" Hunt the missing criterion, and add it to the matrix if warranted.

6. **WRAP-Attain distance (closing):** End with a regret-minimization question: "Bezos's question: looking back at 80, which one would you regret NOT having tried more?" And if useful: "If a friend were in this situation, what would you advise them?"

7. **Final synthesis:** Pull together: the weighted winner + confidence level (are the scores close, is the data certain), the biggest risk from the pre-mortem + its mitigation, intuition–matrix alignment, and the key question the decision hinges on (from the flip test). Leave the decision to the user.

---

## Edge Cases

- **3+ options:** The matrix naturally supports it; add every option as a column. Flip/margin tests apply to the top two options.
- **Hybrid decision (work + personal):** Don't force a single profile; use both domains' criteria together.
- **"Forget it, just tell me":** Per Golden Rule 1, give a reasoned recommendation while leaving the final word to the user, but quickly show the matrix logic so the recommendation isn't ungrounded.
- **User refuses / doesn't know the data:** Mark that criterion `[E]` or leave it "unknown"; report its effect on the result transparently.
- **Only one real option (false dilemma):** If spotted during WRAP-Widen, say it: "There isn't really a dilemma here — option B is already eliminated by X. Is that right?"

## Output Format

Stay in chat, keep it fast. Use a clean table for the matrix. Make the stages visible to the user but don't drown them in academic jargon — only unpack terms like "ROC" or "MAUT" if the user is curious. Wait for the user's answer at each stage; don't dump the whole process in one message.
