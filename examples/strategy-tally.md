# Example run: first-week IA and journey for a habit app (Strategy mode)

A real run of `/ux-strategy` in Claude Code in Strategy mode: no screens yet, only a product brief. For the default Quick fix mode, see [signup-audit.md](signup-audit.md); for a full Audit, see [signup-audit-full.md](signup-audit-full.md).

| | |
|---|---|
| Input | `/ux-strategy Strategy mode: plan the information architecture and the first-week user journey for "Tally", a habit-tracking app MVP. Target: people who have tried habit apps and quit within 2 weeks. Goal: users still logging a habit on day 7. Team of 3, launch in 2 months, no user data yet.` |
| Mode | Strategy (named in the input) |
| Model (from CLI JSON) | `claude-opus-4-7[1m]` (plus a small `claude-haiku-4-5` side call in turn 1) |
| Skill version | 1.2.0 — this commit's `SKILL.md` |
| Claude Code version | 2.1.142 |
| Run date | 2026-10-13 |
| Environment | Fresh isolated install: project-level install from a local clone in an empty temp folder; personal CLAUDE.md, user-level skills and settings, and MCP servers disabled; Bash, web search/fetch, and `AskUserQuestion` disabled; reading the author's user-level `~/.claude` folder and the repo clone denied. `README.md`, `CHANGELOG.md`, and `examples/` were deleted from the install before the run, so the model could not read earlier examples |
| Turns | 3 user turns; stopped at GATE 1 in turn 1 |
| Wall time (from CLI JSON) | 5 min 4 s (53.0 s + 116.5 s + 134.7 s) |

## Run notes

- **"Tally" and the team are fictional.**
- **Output is not edited.** The conversation below is verbatim; assistant replies are shown as blockquotes so their headings don't mix with this page's headings. Tool calls are shown as one-line `[tool call: …]` notes (file paths shortened to the project folder). The final document is the file the skill wrote, copied verbatim.

## Conversation

### Turn 1 — User

> /ux-strategy Strategy mode: plan the information architecture and the first-week user journey for "Tally", a habit-tracking app MVP. Target: people who have tried habit apps and quit within 2 weeks. Goal: users still logging a habit on day 7. Team of 3, launch in 2 months, no user data yet.

### Turn 1 — Assistant

*[tool call: Read `.claude/skills/ux-strategy/references/ux-methods.md`]*

*[tool call: Read `.claude/skills/ux-strategy/references/handoff-map.md`]*

> I've read the skill references. This is Strategy mode — the brief asks for IA and the first-week journey. Strategy mode runs Steps 1–3 and Step 5, with a gate at Step 1. I'll do Step 1 now and stop for your approval before continuing.
>
> ---
>
> # Tally — UX strategy, Step 1: Frame the problem
>
> **Mode**: Strategy — you asked for IA and the first-week journey. Audit and heuristic sweep do not apply (no screens yet).
> **Methods used here**: SCQA (Pyramid Principle, Minto — https://www.barbaraminto.com/), North Star Metric (Amplitude — https://amplitude.com/blog/product-north-star-metric), HEART / Goals→Signals→Metrics (Rodden et al., Google — https://research.google.com/pubs/archive/36299.pdf).
>
> ## SCQA
>
> **Situation.** Tally is a habit-tracking MVP. Team of 3, two-month build window (launch target around 2026-12-13). No user data yet. [User-provided fact]
>
> **Complication.** The target users have already tried habit apps and quit within two weeks. [User-provided fact] So Tally is not competing to be installed — it is competing to still be opened on day 7. Why past apps lost them is not in the brief. [Open question]
>
> **Question (SMART).** *By launch (≈2026-12-13), can Tally's IA and first-week journey get a defined share of new sign-ups to log at least one habit on day 7, measured from install date?*
> The % threshold is deliberately not fixed yet — see metrics below.
>
> **Answer (hypothetical direction, to be tested in Steps 2–3).** Day-7 logging is likely to be shaped more by (a) how fast a user gets to their first successful log, (b) how clearly the app re-surfaces on days 2–6, and (c) how cheap it is to recover after a missed day, than by feature breadth. The MVP's IA and first-week journey should be built around those three, and features that do not serve them should be cut from v1. [AI assumption — this is the direction to validate, not a conclusion]
>
> ## Success metrics — proposed
>
> Your stated goal ("users still logging a habit on day 7") maps cleanly onto a North Star. I'm proposing one North Star + three HEART categories (Retention, Task Success, Engagement). Adoption and Happiness are deferred — too little signal in week 1 to steer by.
>
> **North Star Metric (proposed)**
> *Day-7 Log Rate* = share of new sign-ups in a weekly cohort who log at least one habit on calendar day 7 after sign-up.
> - Captures user value (they are still using it) [AI assumption]
> - Leading indicator of month-1 retention [AI assumption]
> - Directly influenceable by IA and first-week journey choices [AI assumption]
>
> **HEART categories and signals** (Goals → Signals → Metrics)
>
> | Category | Goal (user) | Signal | Metric for the MVP |
> |---|---|---|---|
> | Retention | Still using Tally on day 7 | A log is created on day 7 | **Day-7 Log Rate** (North Star); supporting: Day-3 Log Rate |
> | Task Success | Can create and log a habit without getting stuck | First log happens on day 0 | % of sign-ups whose first habit log occurs within 10 minutes of sign-up; drop-off per onboarding step |
> | Engagement | Comes back more than once in week 1 | Logs on multiple days | Median active-days in days 1–7; share of users with ≥3 active days in week 1 |
>
> Numeric thresholds (e.g. "≥X% Day-7 Log Rate") are **not** proposed here — with no prior data and no cited benchmark, any number would be AI assumption. The team sets a starting target; the metric structure is what Step 1 approves.
>
> ## Open questions I'd like answered or marked "AI assumption, decide later"
>
> 1. Platforms for the MVP (iOS, Android, web)? Affects notification design, which is central to days 2–6. [Open question]
> 2. Is there a working hypothesis about *why* past habit apps lost these users (too much setup, nagging, guilt, feature bloat, something else)? If yes, I'll fold it into Steps 2–3; if no, I'll treat it as an unknown and design the journey to be robust to several of those causes. [Open question]
> 3. Any out-of-scope features already decided (social, streaks, paid tier, reminders)? [Open question]
> 4. Is "a habit" a check (did it / didn't do it), a count, a duration, or user's choice? Shapes the core logging screen and the IA. [Open question]
>
> ---
>
> ## 🚪 Gate 1 — needs your OK before I continue
>
> Please confirm or edit:
>
> - **A. The SMART question** as stated above (Day-7 Log Rate, by launch ≈2026-12-13).
> - **B. The North Star** (Day-7 Log Rate) and **the three HEART categories** (Retention / Task Success / Engagement), with numeric targets deferred to the team.
> - **C. The direction to test in Steps 2–3** (first-log speed, days 2–6 re-surfacing, cheap recovery after a miss) — not a conclusion, a hypothesis.
> - **D. Answers (or "decide later") for the 4 open questions**, especially platform and the habit-type question — those two shape the IA.
>
> Once I have your answer I'll proceed to Step 2 (issue tree) and Step 3 (JTBD, hypothetical journey map, IA recommendation with card-sort/tree-test design), then Step 5 (prioritized roadmap for 2 months with 3 people). I'll stop here until you reply.

### Turn 2 — User

> A, B, C approved. D: 1. You decide. 2. No data yet. 3. Nothing decided — you decide. 4. You decide. Go ahead.

### Turn 2 — Assistant

> # Decisions on the open questions you deferred to me
>
> All four are **AI assumption**; call them out if any are wrong — they shape what follows.
>
> - **D1 Platform.** Mobile-first, **single platform at launch: iOS**. Rationale: three people in two months cannot ship, test, and tune two push-notification stacks and two review processes well; one target lets the first-week journey be designed as one artifact. [AI assumption]
> - **D2 Why past apps lost users.** Treated as unknown; the strategy is designed to be robust against several plausible causes (over-notifying, streak guilt, setup sprawl, feature bloat) rather than optimized for one. [AI assumption]
> - **D3 Out of scope for v1.** Out: streaks, social/sharing, paid tier, analytics dashboards, count/duration habits, cross-device sync. In: single-habit create, daily log, one user-chosen reminder, miss-recovery, basic settings. [AI assumption]
> - **D4 What "a habit" is.** v1 habits are a **binary check per day** (did / didn't), with an optional one-line note. Simplest data model, simplest log UI, consistent with the "fast first log" direction. [AI assumption]
>
> ---
>
> # Step 2 — Issue tree (structure the problem)
>
> **SMART question at the top:** *How do we raise Day-7 Log Rate for new Tally sign-ups, by launch (≈2026-12-13)?* [Approved decision]
>
> Method: issue tree with MECE branches and Day-1 hypotheses (secondary-explainer source: https://www.hackingthecaseinterview.com/pages/issue-trees).
>
> The user's first week has four MECE phases. Each branch carries a hypothesis — testable, falsifiable, actionable — labeled AI assumption since no user data exists yet.
>
> | # | Branch (user phase) | Day-1 hypothesis | Priority |
> |---|---|---|---|
> | B1 | **First log** (sign-up → day 0 first log) | Reducing setup to the minimum (no profile, no categories, 1 suggested habit, pre-filled time) raises the share of sign-ups who complete a first log the same day. [AI assumption] | **80/20** |
> | B2 | **Re-surfacing** (days 1–6, from reminder to log) | A single daily cue at a user-chosen time, with a one-tap log directly from the cue, raises median active-days in week 1 more than adding features would. [AI assumption] | **80/20** |
> | B3 | **Miss recovery** (after any skipped day) | Non-judgmental re-entry (no streak reset, no red marks, "yesterday still editable" for 24 h) raises the share of users who return after a missed day. [AI assumption] | **80/20** |
> | B4 | **Scope guard** (how much Tally asks of the user) | Capping v1 at ≤3 habits per user, binary only, prevents setup sprawl that may have contributed to past abandonment; validate with usage distribution post-launch. [AI assumption] | Supporting |
>
> B1–B3 are the three branches I'd like to concentrate design time on; B4 is a scope rule that governs the other three. [AI assumption]
>
> ---
>
> # Step 3 — Users, journey, and information architecture
>
> ## 3a. JTBD (Jobs-to-be-Done)
>
> Source: https://hbr.org/2016/09/know-your-customers-jobs-to-be-done.
>
> For a user who already tried habit apps and quit within two weeks, the hire-for job is likely:
>
> - **Functional job**: *Help me do a small daily thing consistently without having to think about it.* [AI assumption]
> - **Emotional job**: *Make me feel competent, not guilty, when my behavior is imperfect.* [AI assumption]
> - **Social job**: Likely low or absent for this segment — these users opted out of habit apps once; a social layer is a plausible abandonment cause rather than a draw. v1 ships with no social surface. [AI assumption]
>
> So-what: the emotional job is why B3 (miss recovery, no guilt) sits at the same priority as B1 and B2, and why streaks are out of v1.
>
> ## 3b. Hypothetical journey map — "First week with Tally" (synthetic input)
>
> Title and label per `ux-methods.md`: this map has **no real user research** behind it. Thoughts are paraphrases of a hypothetical persona, not quotes. Source: https://www.nngroup.com/articles/ux-mapping-cheat-sheet/.
>
> | Phase | Day | Actions | Thoughts (hypothetical paraphrase) | Emotion |
> |---|---|---|---|---|
> | Install & first log | 0 | Install → sign up → see one pre-filled habit → tap "Done today" | I've seen apps like this; let's see how long before this one asks me for stuff I don't want to give | Skeptical → mildly surprised that it worked |
> | First return | 1 | Reminder at chosen time → one-tap log from the cue | OK, it reminded me once, didn't nag | Calm |
> | Routine | 2–3 | Reminder → log | Low commitment, that's fine | Neutral |
> | Miss | 4 | Day passes with no log | I knew it, I'll ignore this app now | Guilty, defensive |
> | Recovery | 5 | Open app or next reminder → sees "Yesterday is editable" and today's checkbox, no red marks | Oh — it isn't scolding me | Relief |
> | Continuing | 6 | Reminder → log | This is less work than I expected | Mildly positive |
> | Day 7 | 7 | Reminder → log — **North Star event** | — | — |
>
> Low points are **day 4 (miss)** and **day 0 (the first ask)**. These are the places B1 and B3 intervene. [AI assumption]
>
> ## 3c. Information architecture — recommendation
>
> Flat IA with **3 primary surfaces**, chosen to make the recognition-not-recall load tiny (Nielsen heuristic #6, https://www.nngroup.com/articles/ten-usability-heuristics/):
>
> ```text
> Tally
> ├── Today            ← default surface on open; today's habit(s), log control, yesterday-editable strip
> ├── Habits           ← list, add/edit, pause (max 3 in v1)
> └── Me               ← reminder time, notifications on/off, data export, account
> ```
>
> Why this shape:
> - **"Today" is the home screen.** The core loop is log-today; any screen between the app opening and the log is a tax on B1 and B2. [AI assumption]
> - **"Habits" is separate from "Today".** Create/edit is a weekly action, not a daily one; mixing them crowds the log surface. [AI assumption]
> - **"Me" absorbs everything else** — reminder time, data, account. Deliberately shallow so late scope decisions don't force a re-architecture. [AI assumption]
>
> ### Validation plan for the IA (team runs this)
>
> Both methods are **designed and recommended here**; this skill does not run them.
>
> - **Open card sort** on a set of ~20 candidate labels (e.g. *Reminder time, Pause a habit, Export my data, Edit tomorrow, Notifications, Delete account, Change habit name, Weekly summary*). Goal: find how target users group these before being shown Tally's labels.
> - **Tree test** on the proposed 3-tab tree with 5–7 tasks ("Where would you turn off notifications?", "Where would you edit yesterday's log?", "Where would you add a second habit?"). Measure success rate, directness, time.
> - Participant counts and success thresholds are **not fixed here**; set them with your team. Source for both methods: https://www.nngroup.com/articles/card-sorting-definition/, https://www.nngroup.com/articles/tree-testing/.
>
> ---
>
> # Step 5 — Priorities, recommendation, and a 2-month plan
>
> **Prioritization method.** Evidence-based qualitative priority — not RICE. You have no Reach or Effort numbers from real usage; a RICE table built on invented inputs would mislead more than it helps. (`ux-methods.md` §H.)
>
> ## Recommendation (Pyramid Principle — SCQA already stated in Step 1; source: https://www.barbaraminto.com/)
>
> **Lead sentence:** *Tally should win Day-7 retention by making the first log unavoidably easy, re-surfacing exactly once a day on the user's terms, and treating a missed day as nothing — and should ship nothing in v1 that fights those three.*
>
> **Three reasons (MECE):**
>
> 1. **First log carries most of the weight the team controls in week 1.** A user who does not log on day 0 has no cue, no habit object, and no reason to open the app on day 1. (Branch B1.) [AI assumption]
> 2. **Days 2–6 are won by the reminder, not by the app.** The product has one chance per day to re-enter the user's attention; the reminder is the product on those days. (Branch B2.) [AI assumption]
> 3. **Day 4 is a predictable low point.** How Tally behaves after a miss plausibly separates it from the apps these users have already quit. (Branch B3 + JTBD emotional job.) [AI assumption]
>
> **So-what cuts.** Streaks, social, paid tier, dashboards, count/duration habits — none of these move B1, B2, or B3 in week 1. They are out of v1. [AI assumption]
>
> ## 2-month, 3-person plan
>
> Weeks are indicative; team sets exact dates. Launch target ≈ 2026-12-13. [AI assumption throughout; team adjusts to actual capacity]
>
> | Weeks | Focus | Shippable outcome |
> |---|---|---|
> | 1 | Lock scope on this strategy, run **open card sort** with 10–15 target users (team recruits), draft wireframes for Today / Habits / Me | Approved scope + card-sort results |
> | 2 | **Tree test** the 3-tab IA, finalize copy for the first-log screen and the reminder, decide reminder platform details (iOS notification actions for one-tap log) | IA validated or revised |
> | 3–4 | Build: sign-up + first-log flow (B1), binary log data model, Today surface | First-log flow usable end-to-end |
> | 5 | Build: reminders with one-tap log action from the notification (B2), Habits surface (add/edit/pause, max 3) | Core loop day 0 → day 1 works |
> | 6 | Build: miss-recovery pattern — yesterday-editable 24 h, no streak UI, soft re-entry copy (B3), Me surface | Full week-1 journey walkable |
> | 7 | **Moderated usability test, 5 participants, think-aloud, task: "Set up a habit and come back tomorrow"** (source: https://www.nngroup.com/articles/how-to-conduct-a-heuristic-evaluation/ gives the evaluator count for heuristic eval; a 5-user think-aloud is a separate NN/g pattern — set participant count with the team). Fix top severity-3/4 issues. | Fixes merged |
> | 8 | Instrumentation check (Day-7 Log Rate, Day-3 Log Rate, time-to-first-log, active-days-in-week-1 — HEART source: https://research.google.com/pubs/archive/36299.pdf), submission, launch | v1 live, metrics flowing |
>
> ## What success looks like in the first 4 weeks post-launch
>
> - Day-7 Log Rate: team sets the target now (I will not invent one); watch cohort-over-cohort movement, not an absolute number. [AI assumption on the shape of the measurement; threshold is for the team]
> - Day-3 Log Rate as the earliest leading indicator of Day-7.
> - Time-to-first-log on day 0 — if the median is more than a few minutes, B1 is failing.
> - Share of users who return the day after a missed day — B3's success metric.
>
> ## Validation to run before scaling anything
>
> Each top recommendation gets one cheap test:
>
> | Recommendation | Cheapest test |
> |---|---|
> | 3-tab IA | Tree test, 5–7 tasks (week 2) |
> | First-log flow design (B1) | 5-user think-aloud: sign-up → first log without help (week 7) |
> | Reminder design (B2) | Diary-style check: do users keep the default time? What share disable notifications in the first week? (post-launch, first cohort) |
> | Miss-recovery (B3) | Compare day-after-miss return rate against day-after-log return rate in the first cohorts |
>
> ---
>
> # Handoff (per `handoff-map.md`)
>
> - **Next step: Visual design.** Pass this document plus the 3-tab IA, the first-log flow, and the reminder spec to the visual design step.
> - Also useful for: PRD (approved scope and priorities), and a small research step for the card sort and tree test.
>
> **Should this go to the visual design step next, or do you want me to tighten anything first (e.g. expand the first-log flow into a step-by-step spec, or re-cost the roadmap at a different team shape)?**
>
> ---
>
> ### Evidence summary
>
> Every claim about user behavior, drop-off cause, or impact in this document is a hypothesis (AI assumption), not a measurement. Items marked [User-provided fact] come from your brief and Gate 1 answer. Items marked [Approved decision] are the Gate 1 outcomes. There is no [Real user research] in this document — the card sort, tree test, and 5-user think-aloud are the earliest points real data enters, and your team runs them.

### Turn 3 — User

> No changes. Save the strategy doc as tally-ux-strategy.md.

### Turn 3 — Assistant

*[tool call: Write `tally-ux-strategy.md`]*

*[tool call: Read `tally-ux-strategy.md`]*

> Running the Final check against the saved file. Two issues to fix: (1) a few superlative framings about which moment matters most — not allowed even under an AI-assumption label; (2) one URL in week 7 that isn't in `ux-methods.md`, which the skill forbids.

*[tool call: Edit `tally-ux-strategy.md`]*

*[tool call: Edit `tally-ux-strategy.md`]*

*[tool call: Edit `tally-ux-strategy.md`]*

> Saved to `tally-ux-strategy.md`. Final check caught three things I fixed before reporting done: superlative framings in the recommendation reasons and the journey low-points were rewritten as hypotheses, and the week-7 participant-count link was removed (not in `ux-methods.md`) and the "5" relabelled as a starting point for the team.
>
> Next step per the handoff map is visual design — this doc, the 3-tab IA, the first-log flow, and the reminder spec are what to pass along.

## Final document (`tally-ux-strategy.md`, verbatim)

# Tally — UX strategy

> Mode: Strategy. Methods cited inline. All claims about user behavior, drop-off cause, or impact in this document are hypotheses (AI assumption), not measurements. The earliest real data enters at the card sort, tree test, and 5-user think-aloud in the plan below — your team runs those.

---

## Step 1 — Frame the problem

**Methods used here**: SCQA (Pyramid Principle, Minto — https://www.barbaraminto.com/), North Star Metric (Amplitude — https://amplitude.com/blog/product-north-star-metric), HEART / Goals→Signals→Metrics (Rodden et al., Google — https://research.google.com/pubs/archive/36299.pdf).

### SCQA

**Situation.** Tally is a habit-tracking MVP. Team of 3, two-month build window (launch target around 2026-12-13). No user data yet. [User-provided fact]

**Complication.** The target users have already tried habit apps and quit within two weeks. [User-provided fact] So Tally is not competing to be installed — it is competing to still be opened on day 7. Why past apps lost them is not in the brief. [Open question — deferred]

**Question (SMART).** *By launch (≈2026-12-13), can Tally's IA and first-week journey get a defined share of new sign-ups to log at least one habit on day 7, measured from install date?* [Approved decision]

**Answer (hypothetical direction, to be tested).** Day-7 logging is likely to be shaped more by (a) how fast a user gets to their first successful log, (b) how clearly the app re-surfaces on days 2–6, and (c) how cheap it is to recover after a missed day, than by feature breadth. The MVP's IA and first-week journey should be built around those three, and features that do not serve them should be cut from v1. [Approved decision — direction to test, not a conclusion]

### Success metrics

**North Star Metric.** *Day-7 Log Rate* = share of new sign-ups in a weekly cohort who log at least one habit on calendar day 7 after sign-up. [Approved decision]

**HEART categories and signals** (Goals → Signals → Metrics) [Approved decision]:

| Category | Goal (user) | Signal | Metric for the MVP |
|---|---|---|---|
| Retention | Still using Tally on day 7 | A log is created on day 7 | **Day-7 Log Rate** (North Star); supporting: Day-3 Log Rate |
| Task Success | Can create and log a habit without getting stuck | First log happens on day 0 | % of sign-ups whose first habit log occurs within 10 minutes of sign-up; drop-off per onboarding step |
| Engagement | Comes back more than once in week 1 | Logs on multiple days | Median active-days in days 1–7; share of users with ≥3 active days in week 1 |

Numeric thresholds (e.g. "≥X% Day-7 Log Rate") are deferred to the team. With no prior data and no cited benchmark, any number here would be AI assumption; the metric structure is what is approved.

### Decisions on open questions

All four are **AI assumption**; revisit if any proves wrong — they shape what follows.

- **Platform.** Mobile-first, **single platform at launch: iOS**. Rationale: three people in two months cannot ship, test, and tune two push-notification stacks and two review processes well; one target lets the first-week journey be designed as one artifact. [AI assumption]
- **Why past apps lost users.** Treated as unknown; the strategy is designed to be robust against several plausible causes (over-notifying, streak guilt, setup sprawl, feature bloat) rather than optimized for one. [AI assumption]
- **Out of scope for v1.** Out: streaks, social/sharing, paid tier, analytics dashboards, count/duration habits, cross-device sync. In: single-habit create, daily log, one user-chosen reminder, miss-recovery, basic settings. [AI assumption]
- **What "a habit" is.** v1 habits are a **binary check per day** (did / didn't), with an optional one-line note. Simplest data model, simplest log UI, consistent with the "fast first log" direction. [AI assumption]

---

## Step 2 — Issue tree (structure the problem)

**SMART question at the top:** *How do we raise Day-7 Log Rate for new Tally sign-ups, by launch (≈2026-12-13)?* [Approved decision]

Method: issue tree with MECE branches and Day-1 hypotheses (secondary-explainer source: https://www.hackingthecaseinterview.com/pages/issue-trees).

The user's first week has four MECE phases. Each branch carries a hypothesis — testable, falsifiable, actionable — labeled AI assumption since no user data exists yet.

| # | Branch (user phase) | Day-1 hypothesis | Priority |
|---|---|---|---|
| B1 | **First log** (sign-up → day 0 first log) | Reducing setup to the minimum (no profile, no categories, 1 suggested habit, pre-filled time) raises the share of sign-ups who complete a first log the same day. [AI assumption] | **80/20** |
| B2 | **Re-surfacing** (days 1–6, from reminder to log) | A single daily cue at a user-chosen time, with a one-tap log directly from the cue, raises median active-days in week 1 more than adding features would. [AI assumption] | **80/20** |
| B3 | **Miss recovery** (after any skipped day) | Non-judgmental re-entry (no streak reset, no red marks, "yesterday still editable" for 24 h) raises the share of users who return after a missed day. [AI assumption] | **80/20** |
| B4 | **Scope guard** (how much Tally asks of the user) | Capping v1 at ≤3 habits per user, binary only, prevents setup sprawl that may have contributed to past abandonment; validate with usage distribution post-launch. [AI assumption] | Supporting |

B1–B3 are the three branches to concentrate design time on; B4 is a scope rule that governs the other three. [AI assumption]

---

## Step 3 — Users, journey, and information architecture

### 3a. JTBD (Jobs-to-be-Done)

Source: https://hbr.org/2016/09/know-your-customers-jobs-to-be-done.

For a user who already tried habit apps and quit within two weeks, the hire-for job is likely:

- **Functional job**: *Help me do a small daily thing consistently without having to think about it.* [AI assumption]
- **Emotional job**: *Make me feel competent, not guilty, when my behavior is imperfect.* [AI assumption]
- **Social job**: Likely low or absent for this segment — these users opted out of habit apps once; a social layer is a plausible abandonment cause rather than a draw. v1 ships with no social surface. [AI assumption]

So-what: the emotional job is why B3 (miss recovery, no guilt) sits at the same priority as B1 and B2, and why streaks are out of v1.

### 3b. Hypothetical journey map — "First week with Tally" (synthetic input)

Title and label per `ux-methods.md`: this map has **no real user research** behind it. Thoughts are paraphrases of a hypothetical persona, not quotes. Source: https://www.nngroup.com/articles/ux-mapping-cheat-sheet/.

| Phase | Day | Actions | Thoughts (hypothetical paraphrase) | Emotion |
|---|---|---|---|---|
| Install & first log | 0 | Install → sign up → see one pre-filled habit → tap Done today | I've seen apps like this; let's see how long before this one asks me for stuff I don't want to give | Skeptical → mildly surprised that it worked |
| First return | 1 | Reminder at chosen time → one-tap log from the cue | OK, it reminded me once, didn't nag | Calm |
| Routine | 2–3 | Reminder → log | Low commitment, that's fine | Neutral |
| Miss | 4 | Day passes with no log | I knew it, I'll ignore this app now | Guilty, defensive |
| Recovery | 5 | Open app or next reminder → sees Yesterday is editable and today's checkbox, no red marks | Oh — it isn't scolding me | Relief |
| Continuing | 6 | Reminder → log | This is less work than I expected | Mildly positive |
| Day 7 | 7 | Reminder → log — **North Star event** | — | — |

Day 4 (illustrative miss) and day 0 (the first ask) are plausible low points in this hypothetical journey; they are where B1 and B3 intervene. Validate with real users. [AI assumption]

### 3c. Information architecture — recommendation

Flat IA with **3 primary surfaces**, chosen to make the recognition-not-recall load tiny (Nielsen heuristic #6, https://www.nngroup.com/articles/ten-usability-heuristics/):

```text
Tally
├── Today            ← default surface on open; today's habit(s), log control, yesterday-editable strip
├── Habits           ← list, add/edit, pause (max 3 in v1)
└── Me               ← reminder time, notifications on/off, data export, account
```

Why this shape:
- **Today is the home screen.** The core loop is log-today; any screen between the app opening and the log is a tax on B1 and B2. [AI assumption]
- **Habits is separate from Today.** Create/edit is a weekly action, not a daily one; mixing them crowds the log surface. [AI assumption]
- **Me absorbs everything else** — reminder time, data, account. Deliberately shallow so late scope decisions don't force a re-architecture. [AI assumption]

### 3d. Validation plan for the IA (team runs this)

Both methods are **designed and recommended here**; this skill does not run them.

- **Open card sort** on a set of ~20 candidate labels (e.g. *Reminder time, Pause a habit, Export my data, Edit tomorrow, Notifications, Delete account, Change habit name, Weekly summary*). Goal: find how target users group these before being shown Tally's labels.
- **Tree test** on the proposed 3-tab tree with 5–7 tasks ("Where would you turn off notifications?", "Where would you edit yesterday's log?", "Where would you add a second habit?"). Measure success rate, directness, time.
- Participant counts and success thresholds are **not fixed here**; set them with your team. Sources: https://www.nngroup.com/articles/card-sorting-definition/ , https://www.nngroup.com/articles/tree-testing/.

---

## Step 5 — Priorities, recommendation, and a 2-month plan

**Prioritization method.** Evidence-based qualitative priority — not RICE. You have no Reach or Effort numbers from real usage; a RICE table built on invented inputs would mislead more than it helps. (`ux-methods.md` §H.)

### Recommendation (Pyramid Principle — SCQA stated in Step 1; source: https://www.barbaraminto.com/)

**Lead sentence:** *Tally should win Day-7 retention by making the first log unavoidably easy, re-surfacing exactly once a day on the user's terms, and treating a missed day as nothing — and should ship nothing in v1 that fights those three.*

**Three reasons (MECE):**

1. **Without a day-0 first log, there is no day-1 cue to re-surface against.** A user who does not log on day 0 has no scheduled reminder and no habit object to log against on day 1. (Branch B1.) [AI assumption]
2. **Days 2–6 may hinge more on the reminder than on anything inside the app.** Between reminders, Tally has no recurring presence in the user's attention. (Branch B2.) [AI assumption]
3. **A miss is likely at some point in week 1, and how Tally behaves after one may separate it from the apps these users have already quit.** Validate with cohort-level day-after-miss return rates. (Branch B3 + JTBD emotional job.) [AI assumption]

**So-what cuts.** Streaks, social, paid tier, dashboards, count/duration habits — none of these move B1, B2, or B3 in week 1. They are out of v1. [AI assumption]

### 2-month, 3-person plan

Weeks are indicative; team sets exact dates. Launch target ≈ 2026-12-13. [AI assumption throughout; team adjusts to actual capacity]

| Weeks | Focus | Shippable outcome |
|---|---|---|
| 1 | Lock scope on this strategy, run **open card sort** with target users (team recruits), draft wireframes for Today / Habits / Me | Approved scope + card-sort results |
| 2 | **Tree test** the 3-tab IA, finalize copy for the first-log screen and the reminder, decide reminder platform details (iOS notification actions for one-tap log) | IA validated or revised |
| 3–4 | Build: sign-up + first-log flow (B1), binary log data model, Today surface | First-log flow usable end-to-end |
| 5 | Build: reminders with one-tap log action from the notification (B2), Habits surface (add/edit/pause, max 3) | Core loop day 0 → day 1 works |
| 6 | Build: miss-recovery pattern — yesterday-editable 24 h, no streak UI, soft re-entry copy (B3), Me surface | Full week-1 journey walkable |
| 7 | **Moderated usability test, think-aloud, task: "Set up a habit and come back tomorrow"**. Participant count: starting point of ~5 users for a formative round [AI assumption — team sets the actual number]. Fix top severity-3/4 issues. | Fixes merged |
| 8 | Instrumentation check (Day-7 Log Rate, Day-3 Log Rate, time-to-first-log, active-days-in-week-1 — HEART source: https://research.google.com/pubs/archive/36299.pdf), submission, launch | v1 live, metrics flowing |

### What to watch in the first 4 weeks post-launch

- Day-7 Log Rate: team sets the target; watch cohort-over-cohort movement, not an absolute number. [AI assumption on the shape of the measurement; threshold is for the team]
- Day-3 Log Rate as the earliest leading indicator of Day-7.
- Time-to-first-log on day 0 — if the median is more than a few minutes, B1 may be failing; validate with session recordings or a think-aloud.
- Share of users who return the day after a missed day — B3's success metric.

### Validation to run before scaling anything

Each top recommendation gets one cheap test:

| Recommendation | Cheapest test |
|---|---|
| 3-tab IA | Tree test, 5–7 tasks (week 2) |
| First-log flow design (B1) | 5-user think-aloud: sign-up → first log without help (week 7) |
| Reminder design (B2) | Diary-style check: do users keep the default time? What share disable notifications in the first week? (post-launch, first cohort) |
| Miss-recovery (B3) | Compare day-after-miss return rate against day-after-log return rate in the first cohorts |

---

## Handoff

Per `handoff-map.md`:

- **Next step: Visual design.** Pass this document plus the 3-tab IA, the first-log flow, and the reminder spec to the visual design step.
- Also useful for: PRD (approved scope and priorities), and a small research step for the card sort and tree test.

---

## Evidence summary

- [User-provided fact]: items from the brief and Gate 1 answer.
- [Approved decision]: Gate 1 outcomes (SMART question, North Star, HEART categories, direction to test).
- [AI assumption]: everything else — the four open-question decisions, all hypotheses in the issue tree, the JTBD jobs, the hypothetical journey, the IA rationale, the roadmap, and the priority order.
- [Real user research]: none. First real data enters at the card sort, tree test, and think-aloud listed above.
