# Example run: signup flow audit

A real run of `/ux-strategy` in Claude Code on a mock signup page made for this example.

| | |
|---|---|
| Input | `/ux-strategy Audit the signup flow in signup-mock.html for a habit-tracking app. Goal: more new users finish signup and create their first habit. No analytics yet.` + [`signup-mock.html`](signup-mock.html) |
| Mode chosen by the skill | Audit ("since you asked to audit one existing flow") |
| Model (from CLI JSON) | `claude-opus-4-7[1m]` (plus a 17-token `claude-haiku-4-5` side call in turn 1) |
| Claude Code version | 2.1.142 |
| Run date | 2026-10-12 |
| Environment | Fresh project-level install from local clone at commit 39be617; personal CLAUDE.md, user skills and MCP disabled; stimulus is a mock page made for this example, read as HTML code only |
| Turns | 2 user turns (6 + 2 agent steps) |
| Tokens / cost (from CLI JSON) | Output 17,916 tokens (11,614 + 6,302); cache read 172,447; cache write 49,755; total USD 0.85 (0.603 + 0.243); wall time 4 min 41 s (188.5 s + 92.9 s) |

## Run notes

- **The user answers were written by an AI agent acting as the product team.** "Tally", the team, and the page are fictional.
- **Output is not edited.** The conversation below is verbatim. Tool calls are shown as one-line `[tool call: …]` notes in place of their raw inputs and results (file paths shortened to the project folder). The final report is the file the skill wrote, copied verbatim; nothing was trimmed.
- **What the model could see:** only the HTML/CSS/JS source of `signup-mock.html`. No browser, no screenshot.
- **How it was run:** `claude -p` (then `claude -p --resume <session_id>`) from an empty temp folder, with `CLAUDE_CODE_DISABLE_CLAUDE_MDS=1`, `--setting-sources project --strict-mcp-config --permission-mode acceptEdits --allowedTools "Agent,Task,Read,Write,Edit,Glob,Grep"`. Reading the author's user-level `~/.claude` folder was also denied, because the author has a personal skill with the same name; a probe call confirmed the only `ux-strategy` visible was the freshly installed English one and that no CLAUDE.md was loaded. Bash, web search/fetch, and `AskUserQuestion` were disabled.
- **A first attempt was discarded, and this is why.** In the first attempt `AskUserQuestion` was enabled. In non-interactive `-p` mode nobody can answer that dialog, so both gate questions came back unanswered — and the model continued anyway ("Proceeding with the audit as framed." / "Proceeding with the default lead (F1, F2, F3) and completing Step 5."). That is a real gap: the skill does not say what to do when a gate gets no answer. The run below disables `AskUserQuestion` so gates are asked in plain text, as they would be answered in an interactive session.

## Conversation

### Turn 1 — User

> /ux-strategy Audit the signup flow in signup-mock.html for a habit-tracking app. Goal: more new users finish signup and create their first habit. No analytics yet.

### Turn 1 — Assistant

*[tool call: Glob `**/signup-mock.html`]*

*[tool call: Read `.claude/skills/ux-strategy/references/ux-methods.md`]*

*[tool call: Read `signup-mock.html`]*

*[tool call: Read `.claude/skills/ux-strategy/references/handoff-map.md`]*

I'll run this as **Audit mode** since you asked to audit one existing flow. I inspected `signup-mock.html` (the DOM/code, not a rendered browser session) and will apply Nielsen heuristic evaluation (one AI reviewer) plus a cognitive walkthrough — the two methods that fit a short flow with no analytics. JTBD gets a brief mention because one finding is sequence-related.

Before I write findings, let me confirm **GATE 1** in one line, since the brief already answers it:

- **Core question** (SMART): "What changes to the Tally signup flow would raise the share of new users who complete signup *and* create their first habit, within the next release cycle?"
- **User tasks under review** (no analytics available): T1 Create an account · T2 Complete the profile step · T3 Handle the notification permission · T4 Arrive at a usable dashboard with a first habit.
- **Why these methods**: heuristic evaluation covers the per-screen quality issues; a 4-question cognitive walkthrough catches sequence/visibility gaps on a first-run flow.

I'll produce the audit report now, then pause at **GATE 2** for your OK before I write the prioritized recommendations and handoff.

*[tool call: Write `ux-audit-signup.md`]*

Audit written to `ux-audit-signup.md`. Short version:

**Headline:** The flow asks the user to pay for the account before handing them anything of value, and the parts the user does pay actively punish mistakes. The thing the user came for — tracking a habit — is deferred to an empty dashboard at the very end.

**Severity 4 (catastrophic, 1 finding)**
- **F1** Password field is wiped on validation failure (step 1, line 87). Every failed first attempt forces a full retype of a 10+ character complex password in a masked field. This is the single likeliest conversion killer.

**Severity 3 (major, 5 findings)**
- **F2** `ERR_422: DOB field failed schema validation.` is shown to users (line 105).
- **F3** Password rules hidden until the user fails them.
- **F4** Phone + birthday required with no justification and no apparent product need.
- **F5** Signup gates the first habit; the "done" screen hands users an empty dashboard and a sentence instead of a next action — misses the stated project goal.
- **F6** Notification "Skip" link styled as 11px `#bbb` next to a full-width blue "Allow" — reads as a dark pattern and likely fails contrast.

**Severity 2 (minor, 5 findings)**
- F7 no progress indicator · F8 birthday is free text · F9 no back nav · F10 no social sign-in / no password visibility toggle · F11 done-state has no CTA (overlaps F5).

Cognitive walkthrough on step 2 fails Q3 and Q4 (format ambiguity, no progress visibility).

**GATE 2 — awaiting your OK before Step 5.** Specifically:
1. Does the severity ranking match your read, especially F1 at 4 and F4/F5 at 3?
2. Any findings to drop, merge, or disagree with?
3. Is a 2-step flow (email+password → first-habit) on the table as a direction, or should I recommend fixes that preserve the current 3-step structure?

Once you confirm, I'll produce the prioritized recommendation (Pyramid Principle: conclusion, 3–5 MECE reasons, cheapest next test per recommendation) and the handoff note for the visual-design and PRD steps.

### Turn 2 — User

> Severities look right to us. Keep all findings. Context: we're a team of 3, launching in 2 months, no user data yet. A 2-step flow (email+password, then first habit) is on the table. Go ahead with Step 5.

### Turn 2 — Assistant

Approved. Writing Step 5 now with the two-month, three-person, no-data constraints.

*[tool call: Edit `ux-audit-signup.md`]*

Step 5 appended to `ux-audit-signup.md`. Summary:

**Recommendation (apex):** Before launch, collapse signup to two steps — email+password, then first habit — fix the four mechanical bugs that silently punish users, defer the notification prompt until after the first habit, and ship minimum funnel analytics. Everything else is Phase 1.

**Pre-launch bundle** (fits a 3-person team in 2 months — mostly deletions + one new screen):
1. Delete the profile step (F4) — drops F2 and F8 for free
2. Replace the done screen with "pick your first habit" + 4–6 suggestion chips (F5, F11)
3. Live password-rule checklist that never wipes the field (F1, F3) + show/hide toggle (part of F10)
4. Defer the notification prompt to after the first habit, with a context sentence and equal-weight Skip (F6)
5. Ship 9 funnel events on day one — otherwise launch is blind

**Phase 1 (first 4 weeks post-launch):** Google/Apple sign-in (F10), back nav (F9), progress indicator if not already done (F7).

**Phase 2, data-gated:** Invert the whole flow so the user picks a habit *before* signup (F5 option b). Only run this A/B test if the base funnel plateaus below your target.

**Validation:** One 5-user moderated session before launch, ~2 hours. Then let the funnel drive the next round.

**Handoff:** Parallel to visual design (wireframes for the 2-step flow + error states + deferred notification prompt) and PRD (acceptance criteria tied to `first_habit_created`, event schema as section 8). Three open questions for the team to close before design starts: the suggestion-chip habit list, the events-pipeline choice, and the email-verification policy.

## Final report (`ux-audit-signup.md`, verbatim)

# Tally signup — UX audit

**Mode:** Audit (you asked to review one existing flow).
**Scope reviewed:** `signup-mock.html` — DOM and inline JS. I did not open the file in a browser, so I cannot comment on rendered visuals, animation, timing, or real device behavior. (AI assumption)
**Reviewer count:** 1 AI reviewer. A proper Nielsen heuristic evaluation uses 3–5 evaluators; treat these findings as a single pass. (Sourced fact — https://www.nngroup.com/articles/how-to-conduct-a-heuristic-evaluation/)
**Analytics available:** None. (User-provided fact) Severity ratings therefore combine evidence strength with heuristic judgment, not measured drop-off.

## Headline — top of the pyramid

> **The flow asks the user to pay for the account before it hands them anything of value, and the parts the user does pay actively punish mistakes.** Account creation gates the first habit (the thing the user actually came for), the profile step collects data the app has not justified, errors leak backend jargon, and the password field is wiped on failure. Expect quiet, un-logged drop-off at step 1 (password), step 2 (birthday), and step 4 (empty dashboard). (AI assumption — to be validated with a usability test and funnel analytics.)

## 1 — Framing (SCQA, condensed)

- **Situation**: Tally is a habit-tracking app. New users arrive wanting to track a habit. (User-provided fact)
- **Complication**: The current signup is a 3-step gate (account → profile → notifications → empty dashboard). No analytics, so drop-off is invisible. (User-provided fact)
- **Question**: What in this flow is likely suppressing completion-to-first-habit, and what should be changed first?
- **Answer** (one line, hypothetical): Shorten the gate, repair the broken error handling, and make "first habit created" the end state rather than "account exists". (AI assumption)

## 2 — Methods used and why

| Method | Why chosen | Source |
|---|---|---|
| Nielsen heuristic evaluation (10 principles, severity 0–4) | A short, screen-level flow — this is the exact fit | https://www.nngroup.com/articles/ten-usability-heuristics/ , https://www.nngroup.com/articles/how-to-rate-the-severity-of-usability-problems/ |
| Cognitive walkthrough (4 questions) | First-time user flow — tests whether a stranger can get through | https://www.nngroup.com/articles/cognitive-walkthroughs/ |
| JTBD (brief) | One finding is about ordering — are we giving the user the "job" they hired for? | https://hbr.org/2016/09/know-your-customers-jobs-to-be-done |

RICE is **not** used — no Reach, Impact, or Effort data; qualitative severity-based priority will come at Step 5.

## 3 — Findings (sorted by severity, highest first)

### F1 — Password field is wiped on validation failure · Severity 4 (catastrophic)

| Field | Content |
|---|---|
| Where observed | `signup-mock.html` step 1, `step1` submit handler at lines 83–89. Inspected: inline JS. |
| User task | T1 — Create an account |
| Problem | On any password rule miss, the code sets `document.getElementById('pw').value = ''`. The user types a 10+ character password including uppercase, number, and symbol, misses one rule, and has to retype the whole thing from scratch — in a masked field, with no visibility into what they just typed. |
| Expected impact | High abandonment at step 1, especially on mobile where retyping a long complex password is painful. Users who do retry are more likely to miss again (frustration + lost context). (AI assumption) |
| Related principle | Nielsen #9 Help users recognize, diagnose, and recover from errors; also #3 User control and freedom; also #5 Error prevention |
| Severity + reason | **4** — a hard mechanical punishment on a step 100% of users must pass, with no in-product way to recover gracefully. If any single finding is costing conversions, this one is first in line. |
| Fix | Do not clear the field. Show the rules as a live checklist beneath the field while the user types (rules met flip from grey to green). Keep whatever they typed on error. |
| How to validate | Instrument step-1 submit attempts vs. successful transitions; moderated test with 5 users on mobile, measure retries. |
| Evidence label | AI assumption (code-based) |

### F2 — Birthday error message leaks backend jargon · Severity 3 (major)

| Field | Content |
|---|---|
| Where observed | `signup-mock.html` step 2, line 105: `err.textContent = 'ERR_422: DOB field failed schema validation.'`. |
| User task | T2 — Complete the profile step |
| Problem | On a mis-formatted birthday, the user sees a developer error code and schema terminology. They do not know what format is accepted, do not know what "DOB" or "422" mean, and have no path forward. |
| Expected impact | Users who miss the exact `MM/DD/YYYY` format on first try see a message that reads as "something is broken" rather than "fix your input". Likely silent abandonment or confusion loop. (AI assumption) |
| Related principle | Nielsen #2 Match between system and the real world; #9 Help users recover from errors |
| Severity + reason | **3** — frequent (date formats vary by region) and the message is unrecoverable without guessing the format. Not catastrophic because the user can still re-enter data, unlike F1. |
| Fix | Replace with: "Please enter your birthday as MM/DD/YYYY — for example 04/22/1992." Better: use `<input type="date">` or a masked input that enforces the format. Best: do not ask for birthday here at all (see F4). |
| How to validate | Review session recordings of step 2; a 5-user usability test on step 2 alone. |
| Evidence label | AI assumption (code-based) |

### F3 — Password rules hidden until the user fails them · Severity 3 (major)

| Field | Content |
|---|---|
| Where observed | `signup-mock.html` step 1, placeholder on `#pw` is only "Password" (line 35); rules are only revealed as an error message (line 85) after a failed submit. |
| User task | T1 — Create an account |
| Problem | The user has to invent a password, submit, fail, read the rules, invent a new one. Combined with F1 (field is wiped), this is a two-strike pattern. |
| Expected impact | Users do at least one unnecessary round-trip; some will not come back for a second attempt. (AI assumption) |
| Related principle | Nielsen #5 Error prevention; #6 Recognition rather than recall |
| Severity + reason | **3** — affects every single user who hits step 1. Downgraded from 4 only because by itself it is annoying rather than disqualifying; combined with F1 it becomes catastrophic. |
| Fix | Show rules under the field from the start, as a live checklist that updates as the user types. |
| How to validate | Pair with F1 fix; measure password-attempt-to-success ratio. |
| Evidence label | AI assumption (code-based) |

### F4 — Profile step collects phone and birthday with no justification and no apparent product need · Severity 3 (major)

| Field | Content |
|---|---|
| Where observed | `signup-mock.html` step 2, lines 42–48 (fields) and line 100 ("All fields are required"). |
| User task | T2 — Complete the profile step |
| Problem | A habit-tracking app does not need a phone number or birthday to let the user track habits. Neither field has a "why we need this" explanation. Both are required. No "skip for now" option. |
| Expected impact | A fraction of users will drop out at step 2 on principle — "why does a habit app want my phone?" Even users who comply pay time and friction cost before reaching the thing they came for. (AI assumption) |
| Related principle | Nielsen #8 Aesthetic and minimalist design (ask only what is needed); #10 Help and documentation (explain why). Also JTBD — the user hired this app to track a habit, not to fill out a profile. |
| Severity + reason | **3** — likely a meaningful share of step-2 abandonment, but the fix is a product decision not a bug; cannot be rated 4 without data. |
| Fix | Either (a) delete the profile step entirely for first-run, or (b) make phone and birthday optional and defer them to a later touchpoint, with one-line reasons shown inline. Preferred: delete. |
| How to validate | A/B test with step 2 removed; or a 5-user moderated test asking "would you give this app your phone?" |
| Evidence label | AI assumption + JTBD inference |

### F5 — Account creation gates the first habit · Severity 3 (major)

| Field | Content |
|---|---|
| Where observed | Whole flow: steps 1 → 2 → 3 → done, and the done screen (line 63) ends with an empty dashboard and a text note "add a habit to get started" — not a button, not a prompt. |
| User task | T4 — Arrive at a usable dashboard with a first habit |
| Problem | The user came to the product to track a habit. The app makes them finish signup, agree to notifications, land on an empty dashboard, read a sentence, and then self-start the task they actually came for. The "aha moment" is deferred past the point many will leave. |
| Expected impact | Users who complete signup still may not create a habit — the stated goal of this project — because the final step provides no entry ramp. (AI assumption) |
| Related principle | JTBD; Nielsen #7 Flexibility and efficiency of use; also #1 Visibility of system status (what should I do next?). |
| Severity + reason | **3** — hits the second half of your stated goal ("create their first habit"). Not 4 because the user has at least arrived at the dashboard. |
| Fix | Two options, in order of ambition: (a) replace the final screen with a "Pick your first habit" step (e.g. 4–6 pre-filled suggestions: *Drink water · Walk 10 min · Read before bed · Meditate · Sleep by 11pm · Custom…*) that creates the habit on tap, then lands the user on the dashboard already populated; (b) more ambitious — invert the whole flow: let the user choose a habit **before** signup, then ask for email/password to "save" it. |
| How to validate | After shipping (a): measure the share of signups that create a habit in the first session. After shipping (b): A/B test habit-first vs. signup-first on completion-to-first-habit. |
| Evidence label | AI assumption + JTBD inference |

### F6 — Notification "Skip" link is styled as a dark pattern · Severity 3 (major)

| Field | Content |
|---|---|
| Where observed | `signup-mock.html` step 3, line 18 CSS: `.skip` is `11px`, color `#bbb` (light grey), aligned right. The "Allow notifications" button is full-width, blue, 16px. |
| User task | T3 — Handle the notification permission |
| Problem | The declining-consent option is visually suppressed. On a short card against a light background, 11px `#bbb` text can approach WCAG AA failure for small text (manual check of the color values in the CSS — I did not render the page). Users with low vision or in bright light may not see the skip option. This also removes informed consent — the user is nudged into permissions. |
| Expected impact | Some users tap Allow because Skip is near-invisible; others reach for a browser back button, do not find one (see F9), and bail. (AI assumption) |
| Related principle | Nielsen #3 User control and freedom; also informed-consent / deceptive-pattern concerns |
| Severity + reason | **3** — ethical and legal risk (consent-based permissions regimes), plus practical risk of users abandoning when they cannot find the escape hatch. |
| Fix | Give Skip the same visual weight (text button) at normal body size, placed below Allow. Also add one line of context on why Tally wants notifications (e.g. "To remind you about your habits at the time you pick."). |
| How to validate | Compare Allow rates before/after; monitor step-3 → done rate; separately, run a quick accessibility pass on contrast and tap target size. |
| Evidence label | AI assumption (code-based). Contrast claim: manual check of CSS color values, not a WCAG certification. |

### F7 — No progress indicator across the 3 steps · Severity 2 (minor)

| Field | Content |
|---|---|
| Where observed | Entire flow — no step counter, no bar, no "1 of 3". |
| User task | All of T1–T4 |
| Problem | Users do not know how much is left. First-run flows are a classic place for a step indicator; its absence makes a 3-step flow feel open-ended. |
| Expected impact | A small reduction in persistence through step 2. (AI assumption) |
| Related principle | Nielsen #1 Visibility of system status |
| Severity + reason | **2** — a known pattern gap, but modest cost each. |
| Fix | Add "Step 1 of 2" / "Step 2 of 2" (assuming F4 shrinks the flow) or a simple progress bar. If you keep three steps, use "Step 1 of 3". |
| How to validate | Standard first-run usability test. |
| Evidence label | AI assumption (code-based) |

### F8 — Birthday is a free-text field, not a date picker · Severity 2 (minor)

| Field | Content |
|---|---|
| Where observed | Step 2, line 46: `<input id="bday" type="text" placeholder="MM/DD/YYYY">`. |
| User task | T2 |
| Problem | Free text makes format errors inevitable (see F2). Also no autofill, no keypad hints on mobile. |
| Expected impact | Avoidable errors on step 2 and extra friction on mobile. (AI assumption) |
| Related principle | Nielsen #5 Error prevention; #4 Consistency with platform conventions |
| Severity + reason | **2** — easy to fix, modest-but-reliable gain, assuming F4 does not remove the field entirely. |
| Fix | Use `<input type="date">`, or if the field survives F4, replace with a masked input and inline example. |
| How to validate | Mobile usability test. |
| Evidence label | AI assumption (code-based) |

### F9 — No "back" navigation and no edit path · Severity 2 (minor)

| Field | Content |
|---|---|
| Where observed | Each step is a form with only a "Continue" button; nothing navigates backward. |
| User task | All |
| Problem | A user who realizes they mistyped their email on step 1 after reaching step 2 cannot fix it inside the flow. Browser back on step 1 would also lose the typed password (see F1). |
| Expected impact | Edge-case abandonment and user frustration; harder to recover from a wrong turn. (AI assumption) |
| Related principle | Nielsen #3 User control and freedom |
| Severity + reason | **2** — low frequency, moderate impact. |
| Fix | Add a Back control to steps 2 and 3; preserve previously entered values. |
| How to validate | Usability test with a planted "fix your email" task. |
| Evidence label | AI assumption (code-based) |

### F10 — No social sign-in (Google / Apple), no password visibility toggle, no autofill-friendly phone input · Severity 2 (minor)

| Field | Content |
|---|---|
| Where observed | Step 1 — only email + password form; `#pw` has no show/hide toggle; step 2 phone is a plain `tel` input. |
| User task | T1, T2 |
| Problem | For a mobile-first consumer app, lack of social sign-in adds friction and is now out of pattern. No password visibility toggle compounds F1 by making the retyped password impossible to verify. |
| Expected impact | Modest, but these are table-stakes conveniences in 2026. (AI assumption) |
| Related principle | Nielsen #7 Flexibility and efficiency; #4 Consistency with platform conventions |
| Severity + reason | **2** — not a blocker, but reasonable return on effort. |
| Fix | Add Google / Apple sign-in on step 1. Add an eye-icon show/hide on the password field. Use `inputmode="tel"` and `autocomplete="tel"` for phone if the field survives F4. |
| How to validate | Measure share of signups via social vs. email after launch. |
| Evidence label | AI assumption (code-based) |

### F11 — "Done" state ends on an empty dashboard with no CTA · Severity 2 (minor; overlaps F5)

| Field | Content |
|---|---|
| Where observed | Step `done`, lines 61–64. The copy "Welcome to Tally. Your dashboard is empty — add a habit to get started." is a paragraph, not an action. |
| User task | T4 |
| Problem | After completing signup, the user is left to self-start. No button, no suggestion, no illustration pointing at anything. |
| Expected impact | A share of users close the tab/app here. (AI assumption) |
| Related principle | Nielsen #1 Visibility of system status; #7 Flexibility & efficiency |
| Severity + reason | **2** on its own; combined with F5 it becomes **3**. Listed separately so the fix can ship even if F5 is deferred. |
| Fix | Replace the paragraph with a primary "Add your first habit" button, optionally alongside 4–6 suggestion chips. |
| How to validate | After shipping: measure first-habit-created rate within 60 seconds of landing on done. |
| Evidence label | AI assumption (code-based) |

## 4 — Cognitive walkthrough on step 2 (profile)

Applied to a first-time user whose task is "finish signup so I can start tracking a habit".

| Q | Answer | Note |
|---|---|---|
| Q1 Will the user try the right action? | Maybe. | Some users will balk at "phone number" on a habit app and stop here (F4). |
| Q2 Will the user notice the correct action? | Yes. | Fields and the Continue button are visible. |
| Q3 Will the user associate the action with the result? | No for birthday. | "MM/DD/YYYY" as text input means users may use dashes, dots, 2-digit year, or write it out (F8, F2). |
| Q4 Will the user see progress? | **No.** | No step indicator, no "step 2 of 3" (F7). User does not know if there's a step 3, 4, or 10. |

Two of four questions fail. Fixing F4, F7, F8, F2 would turn all four to Yes.

## 5 — GATE 2 — approved

- All 11 findings kept at the severities listed above. (Approved decision)
- A 2-step flow (email+password → first habit) is on the table. (Approved decision)
- Constraints: team of 3, launching in 2 months (~2026-12-12), no analytics yet. (User-provided fact)

Those constraints change what matters: effort is now semi-known (so Impact/Effort is useable), and the lack of analytics is itself a problem — launching blind means you cannot learn from the launch. Instrumentation gets promoted to a pre-launch recommendation.

## 6 — Recommendation (Pyramid Principle)

> **Before launch, collapse signup to two steps — email+password, then first habit — fix the four mechanical bugs that silently punish users, defer the notification prompt until after the first habit, and ship minimum funnel analytics. Everything else is Phase 1.**

Five reasons, MECE:

1. **The profile step is deadweight.** Phone and birthday have no stated product purpose and no justification shown to the user (F4). Deleting the step also eliminates F2 (jargon error) and F8 (free-text date) at zero cost.
2. **The last screen must be the first habit, not an empty dashboard.** The project goal is signup *and* first-habit — the current done-screen gives up at the halfway line (F5, F11). Replace "You're all set!" with "Pick your first habit" and 4–6 suggestion chips.
3. **Password handling actively drives users out.** F1 (field wiped on error) is the single likeliest conversion killer; F3 (hidden rules) compounds it. Both fix together as a live-rule checklist that preserves input.
4. **The notification permission has no business running during signup.** Users have not seen their first habit yet, so there is no context for why Tally wants to notify them. Defer the prompt until after the first habit is created, where the ask makes sense. This also sidesteps F6 (dark-pattern skip link).
5. **Launching without analytics means launching blind.** With 3 people and 2 months, you cannot run a user study every sprint — you need event data to drive what to fix next. Ship the minimum funnel on day one.

## 7 — Priority and sequencing (Impact/Effort)

Effort estimates are rough. (AI assumption — confirm against your actual sprint capacity.)

| # | Finding(s) | Impact | Effort | Phase |
|---|---|---|---|---|
| 1 | F4 Remove profile step entirely | High | Low (delete) | **Pre-launch** |
| 2 | F5 + F11 Make "pick first habit" the second (and final) step | High | Medium (needs a suggestion list + create-habit wiring) | **Pre-launch** |
| 3 | F1 + F3 Live password-rule checklist; never wipe the field | High | Low | **Pre-launch** |
| 4 | F6 Defer notification prompt to after first habit; context sentence + equal-weight skip | Medium | Low | **Pre-launch** |
| 5 | Minimum funnel analytics (event list below) | High (indirectly — unlocks learning) | Low–Medium | **Pre-launch** |
| 6 | F7 "Step 1 of 2" indicator on step 1 | Low | Low | Pre-launch if cheap; else Phase 1 |
| 7 | F10 Password show/hide toggle | Medium | Low | **Pre-launch** (bundles with F1/F3) |
| 8 | F10 Google / Apple sign-in | Medium–High | Medium (OAuth setup, store review) | Phase 1 (first 4 weeks post-launch) |
| 9 | F9 Back navigation | Low | Low | Phase 1 |
| 10 | F2, F8 | — | — | **Dropped** (profile step is removed) |
| 11 | F5 option (b) — invert the whole flow so the user picks a habit *before* signup | High (hypothetical) | High | Phase 2, only if post-launch funnel data warrants it |

## 8 — Minimum instrumentation (ship on day one)

Nine events, all with timestamp + anonymous session id:

```
signup_started
signup_email_submitted         { ok: bool, error_type?: 'invalid_email' | 'password_rule_miss' }
signup_password_rule_miss      { rules_missed: [...] }      // fires inside live checklist
account_created                 { method: 'email' | 'google' | 'apple' }
first_habit_screen_viewed
first_habit_created             { source: 'suggestion' | 'custom', suggestion_id?: string }
first_habit_skipped             // if you decide to allow this
notification_prompt_shown       // post-first-habit
notification_prompt_response    { response: 'allow' | 'skip' | 'dismiss' }
```

Primary funnel to watch: `signup_started → account_created → first_habit_created`. Define launch success as the ratio of the last to the first. (AI assumption — the team should set the target number.)

## 9 — Next validation (cheapest test per recommendation)

| Recommendation | Cheapest validation |
|---|---|
| 2-step flow (reasons 1 + 2) | One moderated session with 5 recruits before launch, ~2 hours. Pre-launch test is far cheaper than finding out from the funnel. (Sourced fact — https://www.nngroup.com/articles/how-to-conduct-a-heuristic-evaluation/ ; NN/g's related "5 users" guidance: https://www.nngroup.com/articles/why-you-only-need-to-test-with-5-users/) |
| Password rules + no-wipe (reason 3) | Watch `signup_password_rule_miss` and `account_created` ratio for the first 500 signups. |
| Deferred notification prompt (reason 4) | `notification_prompt_response.allow` rate; compare qualitatively to industry benchmarks after the first 500. |
| Instrumentation (reason 5) | Smoke-test every event in staging before cutover. |
| F5 option (b), habit-first invert (Phase 2) | A/B test at ~6–8 weeks if base `signup_started → first_habit_created` plateaus. Only run it if the plateau is clearly below target — do not spend scarce engineering on this if the simple fix already clears the bar. |

## 10 — Handoff

Confirm with the team: this goes next to **visual design** and **PRD** in parallel, not to a business review. (The findings are tactical fixes for a product already heading to launch, not a strategic redirection.)

**Visual design step — needs:**
- Wireframe for the new 2-step flow: step 1 (email + password with live-rule checklist, show/hide toggle, optional "Step 1 of 2" indicator), step 2 (first-habit picker with 4–6 suggestion chips + "Custom habit…" + Save).
- Error-state spec for step 1 (field preserved; rules flip to green as met).
- Post-first-habit notification prompt: card with context sentence, equal-weight Allow and Skip buttons.
- Pass through the full findings list so the designer can see what each decision is addressing.

**PRD step — needs:**
- Acceptance criteria for "signup complete": `account_created` fires.
- Acceptance criteria for the real goal: `first_habit_created` fires in the same session.
- Event schema (section 8) as the source of truth for engineering + analytics.
- Phase 1 scope locked (social sign-in, back nav, progress indicator) so it does not leak into Phase 0.

**What is explicitly out of scope here:**
- Visual polish, colors, typography — visual design step.
- Store listing, pricing, retention features past day 1 — separate strategy work.
- The habit-first invert (F5 option b) — Phase 2 at the earliest, data-gated.

**Open questions** for the team to close before design starts:
- What 4–6 habits do you want pre-filled as suggestions on the first-habit step? (These are the first thing a brand-new user will see — treat the list as a product decision, not a design decision.)
- Does engineering already have an events pipeline (Amplitude, Mixpanel, PostHog, self-hosted)? If not, picking one is a Phase 0 task on the critical path.
- Is email verification required before the first habit, or deferred? The current mock does not address this. Recommend: defer verification to a background email with a magic link, do not block the first habit.

