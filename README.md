# /ux-strategy — UX Audit and Strategy for Claude Code

**For product managers, designers, and founders: get a UX audit or strategy report in which every finding is tied to evidence, a severity, a priority, and the next test that would confirm it.**

## Example

Excerpt from an audit of a 3-step signup mock (quoted from the run, shortened to fit):

```text
Mode: Audit — Steps 1 → 4 → 5.
Reviewer caveat: one AI reviewer reading the DOM/JS of signup-mock.html only —
  no rendered page, no accessibility tooling, no real users.

GATE 1 — please confirm before I run the heuristic pass   (it stopped and waited)

F1 — Password field wiped on validation failure · Severity 4
  Where:            signup-mock.html:87 — document.getElementById('pw').value = '';
  Expected impact:  Perceived hostility at the first interaction. May contribute
                    to T1 attrition; validate.
  How to validate:  5-user think-aloud pre-launch (count attempts to pass Step 1);
                    post-launch, log Step 1 attempt counts.
  Evidence:         Sourced fact (code); expected impact AI assumption.

F16 — No SSO · Severity 1
  Apple's App Store rules may require Sign-in-with-Apple alongside; AI assumption,
  verify current App Store guidelines before implementing.
```

Full conversation and report: [examples/signup-audit.md](examples/signup-audit.md). Real run in Claude Code on a mock page (the run notes list what still deviates).

## Install

**Option A — via the [skills CLI](https://github.com/vercel-labs/skills):**

```bash
npx skills add laymom/consulting-ux-strategy --skill ux-strategy
```

This copies `SKILL.md` together with `references/` (the methods file the skill reads first). For Claude Code at project level it lands in `./.claude/skills/ux-strategy/`.

**Option B — manual clone (user-level, all projects):**

macOS / Linux:

```bash
git clone https://github.com/laymom/consulting-ux-strategy.git ~/.claude/skills/ux-strategy
```

Windows (PowerShell):

```powershell
git clone https://github.com/laymom/consulting-ux-strategy.git "$HOME\.claude\skills\ux-strategy"
```

**First run:** start a new Claude Code session and type `/ux-strategy` followed by your request.

**Optional batch agent (Claude Code only).** `agents/ux-strategist.md` runs the same methods without gates, for another workflow to call or to run several lenses in parallel. You don't need it for normal use, and neither install option above activates it. Install it separately:

macOS / Linux:

```bash
mkdir -p ~/.claude/agents
cp ~/.claude/skills/ux-strategy/agents/ux-strategist.md ~/.claude/agents/
```

Windows (PowerShell):

```powershell
New-Item -ItemType Directory -Force "$HOME\.claude\agents" | Out-Null
Copy-Item "$HOME\.claude\skills\ux-strategy\agents\ux-strategist.md" "$HOME\.claude\agents\"
```

(If you installed with Option A, copy from `./.claude/skills/ux-strategy/agents/` instead.) The agent looks for the skill files at the path given in its brief, then `./.claude/skills/ux-strategy/`, then `~/.claude/skills/ux-strategy/`.

## Copy-paste input example

```text
/ux-strategy Audit our signup flow (3 screens: email → password → profile).
Users: first-time visitors from a mobile ad.
Goal: more people finish signup.
What I have: screenshots attached; no analytics or user research yet.
Constraint: we can change copy and field order this sprint, not the backend.
```

The more of these you include (screens or flow, users, goal, available data, constraints), the fewer questions it asks. The report is written in the language of your input.

## What happens

It picks a mode from your brief and tells you which one and why:

| Mode | When | You get |
|---|---|---|
| **Audit** | Existing screens, a prototype, or key flows | Issues for the main user tasks, each with where it was observed, the problem, expected impact, related principle, severity 0–4 with a reason, a fix, and how to validate it |
| **Strategy** | IA, journeys, and priorities for a product or feature | A strategy doc: direction, information architecture, journey, priorities |
| **Full** | Only when you ask for end-to-end work, such as a new MVP | All five steps below |

The five steps, with two places where it stops for your OK:

```text
1. Frame the problem (SCQA) and agree on metrics or user tasks   [your OK]
2. Structure it (issue tree, MECE, Day-1 hypotheses)
3. Understand users (JTBD, empathy and journey maps, IA)
4. Diagnose (Nielsen heuristics, severity 0–4, cognitive walkthrough)   [your OK]
5. Prioritize, recommend (answer first), name the next validation, hand off
```

Each mode uses only the steps and methods that fit, and the report says why each method was chosen. A small screen check skips the North Star metric and the full issue tree.

Every claim carries one evidence label: **User-provided fact**, **Sourced fact** (with link), **Real user research**, **AI assumption**, **Synthetic persona response**, **Approved decision**, or **Open question**.

The skill produces strategy and diagnosis documents, not visual mockups. An HTML viewer for the report is available on request; that is a way to read the report, not a product screen design.

## How it differs

- **Findings are traceable.** Each issue records where it was observed, which principle it breaks, why it got its severity, and how to check it, so you can disagree with specific points.
- **Observation is kept apart from inference.** A screen review is not behavior data, so the report doesn't claim drop-off causes or conversion lift from one. If only code was read, it says so instead of claiming a visual inspection.
- **Methods are named and cited.** It uses consulting-style methods — Double Diamond (UK Design Council), Nielsen's heuristics and severity scale (NN/g), JTBD (Christensen), issue trees and MECE, the Pyramid Principle (Minto), HEART (Google), RICE (Intercom) — from [`references/ux-methods.md`](references/ux-methods.md). Links marked "secondary explainer" there are not from the original author.
- **Priority matches the data.** With thin data it gives an evidence-based qualitative priority instead of a numeric ranking. RICE inputs that aren't real team numbers are marked as estimates.

## Limits · cost · compatibility

- **It does not replace real user research or a consultant.** Card sorts, tree tests, and usability tests are designed and recommended here; your team runs them with real users.
- **One AI reviewer.** A heuristic evaluation normally uses 3–5 evaluators ([NN/g](https://www.nngroup.com/articles/how-to-conduct-a-heuristic-evaluation/)); this is a single pass.
- **Synthetic input stays synthetic.** Journey or empathy maps built from simulated persona responses are labeled "hypothetical (synthetic input)".
- **Accessibility:** if it comments on accessibility, it states the scope and tool used. It is not a WCAG certification.
- **Cost:** the skill runs in your current session. The optional agent runs as a separate subagent and uses additional tokens, more if you run several in parallel.
- **Compatibility:** tested in Claude Code only. The skills CLI can install `SKILL.md` for other agents, but running it there is untested. The batch agent is Claude Code only.

## Related tools

Small, independent tools by the same author. Use any of them on its own; none requires the others.

- [multi-agent-brainstorm](https://github.com/laymom/multi-agent-brainstorm) — explore and compare ideas from 5 perspectives
- [synthetic-focus-group](https://github.com/laymom/synthetic-focus-group) — explore reactions and risky assumptions with simulated personas
- [ai-prd-generator](https://github.com/laymom/ai-prd-generator) — turn an idea into a PRD with scope and acceptance criteria
- [consulting-ux-strategy](https://github.com/laymom/consulting-ux-strategy) — audit flows, information architecture, and usability, then prioritize fixes

When passing output between them, keep the evidence labels (User-provided fact, Sourced fact, Real user research, AI assumption, Synthetic persona response, Approved decision, Open question). Simulated persona responses stay labeled as synthetic in every downstream document.

## Sources · license · changelog

- Methods and sources: [`references/ux-methods.md`](references/ux-methods.md) (links checked 2026-10-12)
- License: MIT — see [LICENSE](LICENSE)
- Changes: [CHANGELOG.md](CHANGELOG.md)

Repository layout:

```text
SKILL.md                    # the skill (/ux-strategy)
references/ux-methods.md    # method definitions, scales, sources
references/handoff-map.md   # what comes before and after this skill
agents/ux-strategist.md     # optional batch agent (Claude Code only)
examples/                   # a real run: mock page + conversation + report
```

Built by **Sophia in Korea — a non-engineer who ships agents anyway.**
