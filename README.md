# /ux — Consulting-Grade UX Strategy & Diagnosis

A Claude Code skill that does the **UX thinking that happens *before and across* the pixels** — information architecture, user journeys, heuristic diagnosis, prioritization, and strategy — using the **actual methodologies that UX and strategy consulting firms use**, each cited to its primary source.

It does **not** produce visual mockups. It produces **strategy and diagnosis documents**, then hands off to a visual-design step. Think of it as the consultant who frames the problem and tells you what to build and in what order — not the one who picks the colors.

## Why it's different

Most "UX review" prompts give you vibes. This one is anchored to named, sourced methods:

- **Double Diamond** (UK Design Council) · **Design Thinking** (IDEO)
- **Heuristic Evaluation** with Nielsen's 10 heuristics + **Severity 0–4** rating (NN/g)
- **Cognitive Walkthrough** · **UX Maturity model** (NN/g)
- **JTBD** (Christensen) · **Empathy / Journey / Service maps**
- **Issue trees + MECE** · **Hypothesis-driven analysis** (McKinsey)
- **Pyramid Principle / SCQA** (Barbara Minto) for answer-first communication
- **HEART + North Star metrics** · **RICE / Impact-Effort** prioritization
- **McKinsey 7-step** problem-solving as the spine

Every method in [`references/ux-methods.md`](references/ux-methods.md) carries its **source URL**, so the skill cites as it works instead of inventing frameworks from memory. That file is useful as a standalone consulting-methods reference even if you never run the skill.

### The DNA

> **Think bottom-up, communicate top-down.** Analysis starts from data and users; every report leads with the recommendation (Pyramid Principle, answer-first).

## How it works

The skill compresses the McKinsey 7-step process into a 5-step UX flow, with human gates at steps 1 and 4:

```text
/ux <product, screen, or problem>
   │
   ├─ 1. Frame the problem + SCQA + success metrics        🚪 gate
   ├─ 2. Structure it: issue tree, MECE funnel, Day-1 hypotheses
   ├─ 3. Understand users: JTBD · empathy / journey maps · IA (card sort, tree test)
   ├─ 4. Diagnose: heuristic eval (Nielsen 10 + severity 0–4) · cognitive walkthrough   🚪 gate
   └─ 5. Prioritize (RICE / Impact-Effort) + answer-first recommendation + handoff
```

### Three modes

| Mode | When | Output |
|------|------|--------|
| **Audit** | "review this existing screen" | heuristic report (problems + severity + fixes) |
| **Strategy** | "what should we build, in what order" | UX strategy doc (vision · IA · journey · roadmap) |
| **Full** | a new MVP, end to end | full Double Diamond, all 5 steps |

## Skill + Agent (two surfaces)

- **`SKILL.md`** — interactive, with gates; you ping-pong through the steps.
- **`agents/ux-strategist.md`** — an isolated batch agent. Same methods, no gates; another workflow hands it a brief and it returns a full diagnosis in one shot (good for "run 3 UX lenses in parallel").

## Installation

```bash
cp -r consulting-ux-strategy/SKILL.md ~/.claude/skills/ux-strategy/SKILL.md
cp -r consulting-ux-strategy/references ~/.claude/skills/ux-strategy/references
cp consulting-ux-strategy/agents/ux-strategist.md ~/.claude/agents/ux-strategist.md
```

Restart Claude Code, then type `/ux`.

## Usage

```text
/ux our onboarding completion rate is low — diagnose and prioritize fixes
/ux design the information architecture for a new habit-tracking app
```

## Handoff

This skill sits in the middle of a value chain. It receives from a PRD/spec step, and hands off to a visual-design step. For the **qualitative user-feedback** input (empathy/journey maps), it pairs well with a synthetic focus group:

→ [**synthetic-focus-group**](https://github.com/laymom/synthetic-focus-group) — run a qualitative focus group with synthetic personas, then feed the verbatim quotes into this skill's empathy/journey mapping.

## License

MIT — see [LICENSE](LICENSE).

## Credits

Built by **Sophia in Korea — a non-engineer who ships agents anyway.** Methodologies anchored to the primary sources cited in `references/ux-methods.md`.
