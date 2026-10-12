---
name: ux-strategy
description: UX audit and strategy. Reviews screens, flows, and information architecture, and plans what to build and how it should flow, using consulting-style methods (Design Council Double Diamond, Nielsen heuristic evaluation with severity 0–4, issue trees and MECE, Pyramid Principle, JTBD, HEART, RICE). Every finding is tied to evidence, severity, priority, and a next validation step. Produces strategy and diagnosis documents, not visual mockups, and hands off to a visual-design step. Use for UX audits, usability reviews, heuristic evaluation, information architecture, user journeys, onboarding or conversion funnels, and UX prioritization. Invoke explicitly with /ux-strategy.
allowed-tools: Read, Write, Edit, Glob, Grep, WebSearch, WebFetch
---

# UX Strategy Skill

Audits and plans UX: what to build, how it should flow, what is broken, and what to fix first. Every finding is tied to evidence, a severity, a priority, and a way to validate it next. Uses consulting-style methods, cited from `references/ux-methods.md`.

Write all output in the language of the user's input.

## What this skill does not do

This skill produces **strategy and diagnosis documents**. It does not produce HTML mockups or pixel designs.

```text
ux-strategy           →  "what, and how it flows"   (this skill: IA, journeys, diagnosis, strategy)
visual design step    →  "how it looks"             (visual mockups — handoff target)
visual review step    →  "is it well made"          (aesthetics, polish)
```

If a request looks like visual design, stop and ask: "This sounds like visual design output — should it go to a design step instead?" A report viewer (e.g. an HTML page that displays this skill's report) is fine on request; it is not a product screen design.

## Core principle

> **Think bottom-up, communicate top-down.**

Analysis starts from data and users; the report always starts with the conclusion (Pyramid Principle, answer-first). **Read `references/ux-methods.md` before starting** — never reconstruct a method from memory.

## Triggers

- UX audit, usability review, heuristic evaluation, "this is hard to use", "people drop off here"
- Information architecture, menu structure, navigation, "where should things go"
- User journeys, flows, onboarding, conversion funnels, "why don't people use it"
- UX strategy, product direction, "what should we build first", prioritization

Explicit invocation: `/ux-strategy`.

## Evidence labels

Label every claim, input, and finding with exactly **one** of these names — no combinations, no extra qualifiers inside the label:

| Label | Use for |
|---|---|
| **User-provided fact** | Stated by the user or in their materials (PRD, analytics they shared) |
| **Sourced fact** | From a source, always cited: a link from `references/ux-methods.md`, or the user's own provided file with a line (e.g. `signup-mock.html:83`). What you directly observed in the provided code or screens is a Sourced fact cited this way |
| **Real user research** | Results of research with real users (interviews, usability tests, analytics, card sorts) that the user provided |
| **AI assumption** | Your own inference or estimate |
| **Synthetic persona response** | Output from simulated personas (e.g. a synthetic focus group). Never treat as real user research |
| **Approved decision** | Something the user confirmed at a gate |
| **Open question** | Unknown; needs an answer or research |

- Never add a URL that is not in `references/ux-methods.md` (web access may be off; never add unverified links). Claims about industry norms, benchmarks, or "in <year>" need such a source; otherwise drop them or label them AI assumption.
- Statements about the scope of the review itself (e.g. "I read the code only") need no label.

## Modes — pick one from the brief

| Mode | Use when | Core output | Skip by default |
|---|---|---|---|
| **Audit** | Review existing screens, a prototype, or key flows | Issue list for the main user tasks (problem, severity, fix, how to validate) | North Star, full issue tree, roadmap |
| **Strategy** | Decide IA, journeys, and priorities for a product or feature | Strategy doc (direction, IA, journey, priorities) | Exhaustive heuristic pass |
| **Full** | Only when the user asks for end-to-end work (e.g. a new MVP from scratch) | All 5 steps below | — |

- Choose the mode from the brief and state it in one line with the reason ("Audit mode — you asked about one signup screen").
- Apply only the methods that fit, and say in one line why each was chosen. A small screen check does not need a North Star or a full issue tree.
- Don't re-ask what the brief already answers. Ask only for what is missing and blocks the work; if the mode is truly ambiguous, ask one short question.

## Read before starting

| File | When |
|---|---|
| `references/ux-methods.md` | **Always**, first. Method definitions, procedures, scales, sources |
| `references/handoff-map.md` | When deciding what comes next |
| The user's PRD / spec | If provided |
| Screens, HTML, screenshots, or code | In Audit mode, read the target |

## Workflow (consulting steps compressed into 5)

Gates at Steps 1 and 4 only. Other steps proceed on their own, each ending with a one-line summary. Audit mode usually runs a light Step 1, then Step 4 and Step 5. Strategy mode usually runs Steps 1–3 and 5.

**Gate rules**
- At a gate, ask, then stop and wait for the user's reply. Do not continue past the gate in the same turn.
- If the brief already answers a gate, state your assumption in one line, then still stop at that gate once.
- If a gate question gets no answer (non-interactive session, a question tool returns nothing), stop and say exactly what approval is needed. Never "proceed with the default".
- Only if the user explicitly said to run without stopping, continue — and label every choice they did not approve as AI assumption.
- Anything you propose that the brief did not state (user tasks such as T1–T4, scope, metrics) is an AI assumption until approved at a gate.

### Step 1 — Frame the problem (SCQA) 🚪 GATE 1
- State the **core question** in one SMART sentence (e.g. "Can we raise onboarding completion within 3 months?").
- **SCQA**: Situation → Complication → Question → Answer (a hypothetical direction).
- **Success metrics**: in Strategy/Full mode, propose a North Star Metric plus 2–3 HEART categories. In Audit mode, name the user tasks being checked instead.
- 🚪 Get the user's OK on the question and metrics/tasks before Step 2 (see Gate rules).

### Step 2 — Structure the problem (issue tree + MECE)
- Break the UX problem into **MECE** stages (e.g. awareness / onboarding / core loop / retention).
- Attach a **Day-1 hypothesis** to each branch, labeled AI assumption (e.g. "Cutting onboarding from 3 steps to 2 may raise completion — to be validated").
- Mark the 2–3 branches that matter most (80/20) for focused analysis.
- Explain the tree in sentences, not just a table.

### Step 3 — Understand users and journeys
Pick what fits (not all):
- **JTBD**: the real job the user hires the product for (functional / social / emotional).
- **Empathy Map** (Says / Thinks / Does / Feels) and **Journey Map** (Phases / Actions / Thoughts / Emotions). If the inputs are synthetic persona responses or AI assumptions, title the map "hypothetical (synthetic input)".
- **IA**: for menu or structure problems, **recommend** card sorting (open / closed / hybrid) and tree testing. The team runs them with real users; this skill designs and recommends.

### Step 4 — Diagnose (heuristic evaluation, cognitive walkthrough) 🚪 GATE 2
Use the scales in `ux-methods.md` exactly.
- **Heuristic evaluation** against Nielsen's 10. Note that this is one AI reviewer, while a heuristic evaluation normally uses 3–5 evaluators.
- **Cognitive walkthrough** (4 questions) for complex new flows.
- Record **every issue** with these fields:

| Field | Content |
|---|---|
| Where observed | Screen / step / element, and what you actually inspected (rendered screen, screenshot, DOM/code, description) |
| User task | The task the user is trying to do |
| Problem | What goes wrong |
| Expected impact | Effect on the user — stated as expected, not measured |
| Related principle | Heuristic number/name or other method, with source |
| Severity + reason | 0–4, and why |
| Fix | Recommended change |
| How to validate | The test or data that would confirm the problem and the fix |
| Evidence label | From the list above |

- Sort by severity, highest first.
- 🚪 Get the user's OK on the findings before Step 5.

**Observation vs. inference rules**
- Keep what you saw on the screen separate from real user behavior data. A screen review is observation plus AI assumption, not behavior data.
- Never claim drop-off causes or conversion lift from a screen review, and never rank causes of drop-off. Say "may contribute to …; validate with …" and point to the data that would confirm it.
- If you only read the DOM or code, don't claim you inspected the visual design. If you cannot see the screen, say so.
- If accessibility comes up, state the scope and tool used (e.g. "manual check of contrast and labels in the code"). Never imply WCAG certification or full conformance.

### Step 5 — Prioritize, recommend, hand off
- **Priority**: with thin data, prefer an evidence-based qualitative priority (severity + evidence label + reason). Use an Impact/Effort 2x2 when effort is roughly known, and RICE only when the team has real Reach and Effort numbers; otherwise mark RICE inputs as estimates (AI assumption).
- **Recommendation** (Pyramid Principle): one-sentence conclusion first → 3–5 MECE reasons → data.
- **So-what test**: cut analysis that does not bear on the recommendation.
- **Next validation**: for each top recommendation, name the cheapest test that would confirm or reject it.
- **Handoff**: follow `references/handoff-map.md` — visual design, PRD, or business review.

## Output format

- Lead with the conclusion in plain sentences, then put tables and scores behind it as support. Don't open with framework names or score tables.
- State the mode and why, and the methods used and why, near the top.
- Cite a source (URL from `ux-methods.md`) whenever you use a method — including where it is applied (e.g. Pyramid Principle/SCQA at the recommendation), once there.
- Separate your opinions from facts with evidence labels.
- If saving a document, use `.md` (and an HTML report viewer only on request) in the user's project folder.

## Final check — run on the finished report before sending

Fix the report until every answer is "yes":

1. Search the report for "conversion", "drop-off", "abandon", "killer", "costing", "likeliest", "will increase". Without real usage data, is every such claim rewritten as a hypothesis ("may contribute to …; validate with …"), with no ranking of drop-off causes?
2. Is every evidence label exactly one of the 7 names? (No combinations such as "AI assumption + JTBD inference". Direct observations of the provided files are Sourced fact with file and line.)
3. Does every Sourced fact link come from `references/ux-methods.md`, or point to the user's own provided file and line? No other URLs.
4. Is every claim about industry norms, benchmarks, or "in <year>" sourced — or removed or labeled AI assumption?
5. Where a method is applied (e.g. Pyramid Principle/SCQA in the recommendation), is its source link cited once there?

## Never

- Produce HTML pixel mockups (→ visual design step)
- Invent method definitions from memory (→ read `ux-methods.md` first)
- Fabricate user test results, analytics, or quotes. Real testing is the team's job; this skill designs and recommends
- Present synthetic persona responses as real user research
- Skip GATE 1 or GATE 2, or continue past an unanswered gate
