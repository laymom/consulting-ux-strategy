# UX and Strategy Methods Reference

> Definitions, procedures, scales, and sources for the methods this skill uses. Read this file before starting work, and cite the source each time you use a method. Do not reconstruct methods from memory. (Links checked: 2026-10-12. Links marked "secondary explainer" are not from the original author or institution.)

Core idea: **"Think bottom-up, communicate top-down"** — analysis starts from data; the report starts from the conclusion.

Not every method applies to every request. Pick the ones that fit the mode and the brief (see SKILL.md), and say why each was chosen.

---

## A. Diverge–converge processes

### Double Diamond (UK Design Council)
Explore the problem (diverge) → define the problem (converge) → explore solutions (diverge) → deliver the solution (converge).

| Stage | Direction | Activities |
|---|---|---|
| Discover | Diverge | Understand without assumptions. Interviews, observation, research |
| Define | Converge | Reframe insights as HMW (How Might We) challenges |
| Develop | Diverge | Generate varied solutions, co-design, prototype |
| Deliver | Converge | Small tests → iterate → launch |

Source: https://www.designcouncil.org.uk/resources/the-double-diamond/

### Design Thinking, 5 stages (Stanford d.school — Hasso Plattner Institute of Design)
Empathize → Define (write a POV: "The user needs ___ because ___") → Ideate (diverge without judgment) → Prototype (cheap and fast) → Test (loop back to empathy). Non-linear and iterative.
Source (secondary explainer): https://ixdf.org/literature/article/5-stages-in-the-design-thinking-process

---

## B. UX diagnosis and evaluation

### Heuristic Evaluation — Jakob Nielsen / NN/g
Experts independently check an interface against 10 principles. **3–5 evaluators** is the recommended range. (This skill is one AI reviewer, so its findings are a single evaluator's pass; say so in the report.)
Procedure: ① select evaluators → ② each evaluates independently for 1–2 hours → ③ each rates severity independently → ④ aggregate and discuss.
Source: https://www.nngroup.com/articles/how-to-conduct-a-heuristic-evaluation/

**Severity Rating 0–4** (a release-decision scale):
- 0 = Not a usability problem
- 1 = Cosmetic (fix if time allows)
- 2 = Minor (low priority)
- 3 = Major (high priority)
- 4 = Catastrophic (must fix before release)

Severity is a judgment that combines frequency, impact, persistence, and market impact into one score. Give the reason with every rating.
Source: https://www.nngroup.com/articles/how-to-rate-the-severity-of-usability-problems/

**Nielsen's 10 heuristics** (checklist):
1. Visibility of system status 2. Match between system and the real world 3. User control and freedom (cancel, undo) 4. Consistency and standards 5. Error prevention 6. Recognition rather than recall (lower memory load) 7. Flexibility and efficiency of use 8. Aesthetic and minimalist design 9. Help users recognize, diagnose, and recover from errors 10. Help and documentation
Source: https://www.nngroup.com/articles/ten-usability-heuristics/

### Cognitive Walkthrough
Walk through each step of a task from a new user's point of view and ask 4 questions. Suited to complex new flows and kiosks (overkill for standard patterns).
Four questions per step: Q1 Will the user try to achieve the right result? Q2 Will the user notice that the correct action is available? Q3 Will the user associate the correct action with the result they want? Q4 After the action, will the user see that progress is being made? Any "No" is a fail.
Source: https://www.nngroup.com/articles/cognitive-walkthroughs/

### UX Maturity, 6 stages (NN/g; an organizational self-assessment tool)
1 Absent → 2 Limited → 3 Emergent → 4 Structured → 5 Integrated → 6 User-Driven. Four factors: Strategy, Culture, Process, Outcomes. Stages cannot be skipped.
Source: https://www.nngroup.com/articles/ux-maturity-model/

---

## C. User and journey tools

### Empathy Map — Says / Thinks / Does / Feels
Collect qualitative research → fill the quadrants → cluster and look for gaps. Used before creating personas, to align the team.
Source: https://www.nngroup.com/articles/empathy-mapping/

### Journey Map — Phases / Actions / Thoughts / Emotions
Traces, in time order, how a specific persona pursues a specific goal. Low points in the emotion curve are candidate intervention points. When the inputs are synthetic persona responses or AI assumptions rather than real user research, label the map (and any empathy map) "hypothetical (synthetic input)".
Source: https://www.nngroup.com/articles/ux-mapping-cheat-sheet/

### Service Blueprint (omnichannel, multi-department services)
Five layers: physical evidence / customer actions / frontstage (visible staff and systems) / backstage (invisible internal work) / support processes. Three lines: line of interaction, line of visibility, line of internal interaction. A follow-on to the journey map.
Source: https://www.nngroup.com/articles/service-blueprints-definition/

### JTBD (Jobs-to-be-Done) — Christensen
"Customers don't buy a product because of their demographics; they hire it to get a job done in a particular circumstance." Three kinds of job: functional, social, emotional.
Example: people don't want a quarter-inch drill, they want a quarter-inch hole (Levitt/Christensen) → design for the result the user is trying to hire the product for.
Source: https://hbr.org/2016/09/know-your-customers-jobs-to-be-done
Drill example source: Christensen, Cook, Hall, "Marketing Malpractice: The Cause and the Cure", HBR 2005 — https://hbr.org/2005/12/marketing-malpractice-the-cause-and-the-cure (full text paywalled; abstract: https://pubmed.ncbi.nlm.nih.gov/16334583/)

---

## D. Information architecture (IA)

### Card sorting — discover mental models (early IA)
Open (users create the group names; for new designs) / Closed (users sort into existing categories; for validation) / Hybrid (some categories given, users may add more).
Source: https://www.nngroup.com/articles/card-sorting-definition/

### Tree testing — validate the IA (after card sorting)
Tasks like "Where would you find X?" on a text-only tree with no visual design. Analyze success rate, time taken, and path (directness).
Source: https://www.nngroup.com/articles/card-sorting-tree-testing-differences/ , https://www.nngroup.com/articles/tree-testing/

> Note: running card sorts and tree tests with real users is the team's job. This skill **designs and recommends** them; it never reports results it did not get.

---

## E. Success metrics

### HEART (Google) + Goals–Signals–Metrics
Happiness (satisfaction, NPS) / Engagement (DAU, session depth) / Adoption (new users, new-feature uptake) / Retention (retention, churn) / Task Success (completion rate, errors, time).
Process: map each category from Goal → user-behavior Signal → quantitative Metric.
Source: Rodden, Hutchinson, Fu, "Measuring the User Experience on a Large Scale", CHI 2010 — https://research.google.com/pubs/archive/36299.pdf (author's overview page: https://kerryrodden.com/heart/)

### North Star Metric (Amplitude)
A single metric that captures the core value customers get. A leading indicator of revenue, something the team can influence, and an expression of user value. Accompanied by 3–5 input metrics.
Source: https://amplitude.com/blog/product-north-star-metric

### NN/g UX Strategy, 3 components
Vision (user-centered future state, differentiation) + Goals & Measures (measurable goals and KPIs, tied to the business model) + Plan (a roadmap per goal). Supported by People, Process, Tools (DesignOps).
Source: https://www.nngroup.com/articles/ux-strategy/

---

## F. Problem structuring (strategy consulting)

### MECE
Mutually Exclusive (no overlaps), Collectively Exhaustive (together they cover everything). Commonly attributed to Barbara Minto alongside the Pyramid Principle. In practice, refine iteratively to around 80% — a direction to aim for, not something to achieve perfectly.
Source (secondary explainers): https://deckary.com/blog/pyramid-principle-consulting , https://www.betterup.com/blog/minto-pyramid (MECE is not mentioned on Minto's official site, https://www.barbaraminto.com/ , as of the check date)

### Issue tree / hypothesis tree
SMART question at the top → 3–5 MECE branches at level 1 → break down further → check MECE → prioritize 80/20. Once a direction emerges, switch from an issue tree (exploration) to a hypothesis tree ("this is the cause").
Three ways to split: by stakeholder / by process / by formula (revenue = price × volume). Commonly described as a McKinsey-style method.
Source (secondary explainer, case-interview prep site): https://www.hackingthecaseinterview.com/pages/issue-trees

### Hypothesis-driven approach
State a Day-1 hypothesis first (even with thin evidence) → prove or reject it through analysis → if rejected, move straight to the next branch. A good hypothesis is testable, falsifiable, consequential, tied to an action, and not already known. Commonly described as a McKinsey-style method.
Source (secondary explainer, template vendor site): https://slideworks.io/resources/mckinsey-problem-solving-process

---

## G. Communication (Pyramid Principle — Barbara Minto)

Three levels: apex (key recommendation = governing thought) → middle (3–5 MECE arguments) → base (data).

**SCQA** (the opening):
- Situation: the current state the audience already knows
- Complication: what changed, or what threatens it
- Question: the question that follows
- Answer: the apex of the pyramid = your recommendation

**Principles**: Answer first (lead with the conclusion, deductively). So-what test (cut anything that does not bear on the recommendation). Action titles (a slide title states the conclusion, not the topic — "Customer satisfaction overview" ✗ / "Falling satisfaction puts renewal revenue at risk" ✓).
Source: Barbara Minto, *The Pyramid Principle* (book) · https://www.barbaraminto.com/
Secondary explainers: https://deckary.com/blog/pyramid-principle-consulting , https://www.betterup.com/blog/minto-pyramid

---

## H. Prioritization and decisions

| Situation | Tool |
|---|---|
| A handful of items, thin data, or a workshop setting | Evidence-based qualitative priority, or Impact/Effort 2x2 |
| Many items, and real Reach and Effort numbers are available (or Reach varies widely) | RICE |
| Multiple criteria, high stakes | Weighted scoring |

These are rules of thumb, not thresholds. With thin data, prefer an evidence-based qualitative priority (severity + evidence label + reason) over a numeric ranking.

**RICE** = (Reach × Impact × Confidence%) / Effort
- Reach: users affected per quarter / Impact: 3 = massive, 2 = high, 1 = medium, 0.5 = low, 0.25 = minimal / Confidence: high 100%, medium 80%, low 50% / Effort: person-months
- If Reach or Effort are not real numbers from the team, mark them as estimates (AI assumption) and say the ranking is only as good as those estimates.
Source: https://www.intercom.com/blog/rice-simple-prioritization-for-product-managers/

**Value Proposition Canvas** (Strategyzer): pairs the customer profile (Jobs, Pains, Gains) with the value map (Products & Services, Pain Relievers, Gain Creators) to check fit.
Source: https://www.strategyzer.com/library/the-value-proposition-canvas

---

## I. Consulting 7 steps (overall flow — commonly described as a McKinsey-style method)
1 Define (define the problem) → 2 Disaggregate (break it down with an issue tree) → 3 Prioritize (prioritize and prune) → 4 Workplan → 5 Analyze → 6 Synthesize (combine the findings) → 7 Tell the story (communicate persuasively). The SCQA and pyramid used in step 7 come from Minto (see G).
Source (names of the 7 steps): Charles Conn & Robert McLean, *Bulletproof Problem Solving* (Wiley, 2019) — https://www.wiley.com/en-us/Bulletproof+Problem+Solving%3A+The+One+Skill+That+Changes+Everything-p-9781119553021
Reference (2019 McKinsey podcast): https://www.mckinsey.com/capabilities/strategy-and-corporate-finance/our-insights/how-to-master-the-seven-step-problem-solving-process
