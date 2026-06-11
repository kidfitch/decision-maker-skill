# Decision Maker — A Claude Skill

A hybrid decision-making skill for when you're stuck between two (or more) options. It asks **deep questions** and walks you to a **data-driven decision that still honors your intuition** — analysis-heavy (~70%) but never ignoring the gut (~30%).

Built on real decision-science methods, not vibes: Multi-Criteria Decision Analysis (MCDA/MAUT), Rank Order Centroid (ROC) weighting, Kahneman's dual-process theory, Klein's naturalistic decision making, and the Heath brothers' WRAP process.

## What it does

When you say things like *"I'm stuck between two jobs"*, *"which should I pick"*, *"this or that"*, or *"I can't decide"*, the skill runs a structured process:

1. **Triage** — Is the decision reversible? High-impact? Picks a fast or deep path (Bezos's Type 1 / Type 2 doors).
2. **Context & Gut Seal** — Clarifies the options, checks for a hidden third option, calibrates how expert you are in the domain, and seals away your initial gut read *before* analysis (anchoring prevention).
3. **Criteria & Weighting** — Elicits what matters, removes overlapping criteria, has you rank them, then derives weights mathematically via ROC.
4. **Scoring & Reality-Test** — Scores each option with a rationale and a confidence flag (`[K]` known / `[E]` estimate), then asks you to challenge the scores.
5. **Matrix, Sensitivity, Pre-Mortem & Synthesis** — Computes a weighted matrix, runs two robustness tests, runs a pre-mortem, then confronts the result with your sealed gut read. When the number and the gut disagree, it hunts the **missing criterion** — the most valuable output.

It never decides *for* you. It structures the decision so you can see your own best answer.

## Why it's different

Most decision skills just build a scoring matrix. This one:

- **Derives weights from a ranking** (ROC) instead of asking you to invent percentages.
- **Seals your intuition at the start** and confronts it at the end — turning a number/gut conflict into a discovery, not a tiebreaker.
- **Flags estimated vs. known scores** so it never manufactures false precision.
- **Reduces the decision to one question** when the flip test shows it hinges on a single criterion trade-off.
- **Adapts depth** to the stakes, so it won't exhaust you on a small choice.

See [`decision-maker/references/methods.md`](decision-maker/references/methods.md) for the full formulas and academic grounding.

## Installation

### Claude Code
Clone into your skills directory:

```bash
git clone https://github.com/<your-username>/decision-maker.git
cp -r decision-maker ~/.claude/skills/
```

Or place the `decision-maker/` folder anywhere Claude Code discovers skills (project-level `.claude/skills/` or user-level `~/.claude/skills/`).

### Claude.ai / Cowork
Zip the `decision-maker/` folder and upload it via **Settings → Capabilities/Skills → Upload skill** (requires a plan with Skills enabled).

## Structure

```
decision-maker/
├── SKILL.md              # The process: triage + 4 stages, golden rules, edge cases
└── references/
    └── methods.md        # Formulas + rationale: ROC, MAUT, sensitivity tests,
                          # Kahneman/Klein, WRAP, pre-mortem, Type 1/2
```

## How triggering works

The skill appears to Claude with its name + description. Claude consults it when your request matches — typically multi-step decisions between concrete alternatives. Simple yes/no questions or factual lookups won't trigger it (by design).

## License

MIT — see [LICENSE](LICENSE).

## Credits

Methods drawn from published decision science: Daniel Kahneman (*Thinking, Fast and Slow*), Gary Klein (Recognition-Primed Decision model, pre-mortem), Chip & Dan Heath (*Decisive*, WRAP), and the MCDA/ROC literature on surrogate weighting.
