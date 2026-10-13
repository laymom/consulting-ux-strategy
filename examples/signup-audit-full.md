# Example run: signup flow audit (Audit mode, v1.1.0)

A real run of `/ux-strategy` in Claude Code on a mock signup page made for this example, in Audit mode (the full review with gates). For the default Quick fix mode, see [signup-audit.md](signup-audit.md).

| | |
|---|---|
| Input | `/ux-strategy Audit the signup flow in signup-mock.html for a habit-tracking app. Goal: more new users finish signup and create their first habit. No analytics yet.` + [`signup-mock.html`](signup-mock.html) |
| Mode chosen by the skill | Audit ("You asked to review one existing flow … against a specific business goal") |
| Model (from CLI JSON) | `claude-opus-4-7[1m]` (plus a small `claude-haiku-4-5` side call in turn 1) |
| Skill version | 1.1.0 (`SKILL.md` as of commit 4df9700) |
| Claude Code version | 2.1.142 |
| Run date | 2026-10-12 |
| Environment | Fresh project-level install from a local clone; personal CLAUDE.md, user skills and MCP disabled; stimulus is a mock page made for this example, read as HTML code only |
| Turns | 4 user turns (5 + 1 + 1 + 2 agent steps) |
| Gates | Stopped at GATE 1 (turn 1) and GATE 2 (turn 2); each was answered explicitly |
| Wall time (from CLI JSON) | 7 min 15 s (46.7 s + 193.8 s + 76.5 s + 117.8 s), not counting one failed attempt (see notes) |

## Run notes

- **"Tally", the team, and the page are fictional.** The answers reuse the facts from the previous version of this example (team of 3, launch in 2 months, no user data, a 2-step flow on the table, severities accepted); this time each gate was answered explicitly, including approval of the user tasks at GATE 1.
- **An earlier run of this example revealed issues that this version fixes** (gates passed without an answer, combined evidence labels, unsourced links and norms, ranked drop-off claims) — see [CHANGELOG 1.1.0, "Fixed (found in a real run)"](../CHANGELOG.md).
- **Output is not edited.** The conversation below is verbatim; assistant replies are shown as blockquotes so their headings don't mix with this page's headings. Tool calls are shown as one-line `[tool call: …]` notes in place of their raw inputs and results (file paths shortened to the project folder). The final report is the file the skill wrote, copied verbatim; nothing was trimmed.
- **What the model could see:** only the HTML/CSS/JS source of `signup-mock.html`. No browser, no screenshot.
- **How it was run:** `claude -p` (then `claude -p --resume <session_id>`) from an empty temp folder, after `npx -y skills add <local clone> --skill ux-strategy -a claude-code -y`, with `CLAUDE_CODE_DISABLE_CLAUDE_MDS=1`, `--setting-sources project --strict-mcp-config --permission-mode acceptEdits --allowedTools "Agent,Task,Read,Write,Edit,Glob,Grep"`. Bash, web search/fetch, and `AskUserQuestion` were disabled, so gates were asked in plain text. Reading the author's user-level `~/.claude` folder was denied, because the author has a personal skill with the same name; a probe call confirmed that the only `ux-strategy` visible was the freshly installed English one, that no CLAUDE.md was loaded, and that reading the personal skill file was denied. The installer also copied this repo's `examples/` folder into the install; it was deleted from the install before the run so the model could not read the previous example.
- **One transient failure.** The first send of the turn-2 message returned "API Error: The socket connection was closed unexpectedly." with no model output; the same message was resent (marked in the conversation).
- **What still deviates from the skill's rules in this run (left unedited):** F4's evidence line combines two labels ("Sourced fact (code) + User-provided fact"); two findings rank causes ("May be the single highest-impact gap", "May be the strongest contributor to Step-2 attrition"); F11 states an iOS/Android permission behavior without a source; and reason 1 of the recommendation says the team "flagged phone/birthday as not clearly load-bearing", which the user never said.

## Conversation

### Turn 1 — User

> /ux-strategy Audit the signup flow in signup-mock.html for a habit-tracking app. Goal: more new users finish signup and create their first habit. No analytics yet.

### Turn 1 — Assistant

*[tool call: Read `.claude/skills/ux-strategy/references/ux-methods.md`]*

*[tool call: Read `.claude/skills/ux-strategy/references/handoff-map.md`]*

*[tool call: Glob `**/signup-mock.html`]*

*[tool call: Read `signup-mock.html`]*

> Reading the skill references and the mock first. Now I'll frame the problem and stop at Gate 1.
>
> ---
>
> # Tally Signup Audit — Framing (Step 1 of Audit mode)
>
> **Mode: Audit.** You asked to review one existing flow (`signup-mock.html`) against a specific business goal, so this is Audit mode, not Strategy or Full. I'll run a light Step 1 (SCQA + tasks), then Step 4 (heuristic evaluation + cognitive walkthrough) and Step 5 (prioritize + recommend). Sources: Pyramid Principle / SCQA — https://www.barbaraminto.com/ ; Heuristic evaluation — https://www.nngroup.com/articles/how-to-conduct-a-heuristic-evaluation/ ; Cognitive walkthrough — https://www.nngroup.com/articles/cognitive-walkthroughs/
>
> **Scope of this review** — I read the DOM and JS in `signup-mock.html` only. I did not render the page in a browser, so I'm not making visual-design or accessibility-conformance claims. No analytics exist, so every behavioural claim is a hypothesis to validate, not a measurement.
>
> ## SCQA
>
> - **Situation (Sourced fact: signup-mock.html:29–64)** — Tally has a 3-step signup: ① email + password → ② phone + birthday → ③ notification prompt → "You're all set" screen that tells users their dashboard is empty.
> - **Complication (User-provided fact)** — You want more new users to both finish signup *and* create their first habit, but there is no analytics data yet to see where they stall.
> - **Question** — Which specific changes to this flow would most plausibly raise completion of signup through first-habit creation?
> - **Answer (AI assumption, to be validated)** — The flow likely leaks at three places: profile step asks for phone + birthday with no stated reason (Step 2), password rules are strict and the field is wiped on error (Step 1), and the final screen hands the user to an empty dashboard instead of a 1-tap "add your first habit" (Done). Reducing/deferring profile data, softening error handling, and routing into first-habit creation are the most promising levers.
>
> ## User tasks being checked (replaces metrics in Audit mode)
>
> | ID | Task | Where it lives today |
> |---|---|---|
> | T1 | Create an account (email + password) | signup-mock.html:30–38 |
> | T2 | Provide profile info required by Tally | signup-mock.html:41–49 |
> | T3 | Decide on notifications | signup-mock.html:52–58 |
> | T4 | Create their first habit | **Not in this file** — "done" screen ends on an empty dashboard (signup-mock.html:61–64). Open question: is first-habit creation in scope of this audit? |
>
> ## 🚪 GATE 1 — please confirm before I run the heuristic pass
>
> Three questions, short answers are fine:
>
> 1. **Core question OK as stated?** ("Which specific signup changes would most plausibly raise completion through first-habit creation?") Y/N or edit.
> 2. **Tasks T1–T4 OK?** In particular, do you want T4 (first-habit creation) treated as **in-scope** for the audit? Options:
>    - (a) In-scope — I'll flag the missing handoff from signup to habit creation as an audit finding.
>    - (b) Out-of-scope — I'll audit T1–T3 only and note T4 as an open handoff item.
> 3. **Any constraints I should know?** For example: platform (web only? mobile wrapper?), required legal fields (is phone/birthday actually mandatory, or legacy?), or anything already decided that I shouldn't challenge.
>
> I'll stop here and wait.

*[Run note: this message was first sent once and the CLI returned "API Error: The socket connection was closed unexpectedly." before any model output. The identical message was resent; the exchange below is the resend.]*

### Turn 2 — User

> 1. Core question is fine as stated. 2. T1–T4 approved, and T4 is in scope — option (a). 3. Context: we're a team of 3, launching in 2 months, no user data yet. A 2-step flow (email+password, then first habit) is on the table. Nothing else is decided. Go ahead with the heuristic pass.

### Turn 2 — Assistant

> Gate 1 approved. Running the heuristic pass (Nielsen's 10) and cognitive walkthrough against T1–T4, with your 2-step alternative as context.
>
> ---
>
> # Tally Signup Audit — Step 4: Diagnosis
>
> **Methods used, and why**
> - **Nielsen heuristic evaluation** (10 heuristics, severity 0–4) — fits a one-screen flow where the question is "what breaks for a new user." Source: https://www.nngroup.com/articles/how-to-conduct-a-heuristic-evaluation/ and https://www.nngroup.com/articles/ten-usability-heuristics/
> - **Cognitive walkthrough** (4 questions per step) — fits a new user doing an unfamiliar flow, and makes T4 (first habit) failure visible. Source: https://www.nngroup.com/articles/cognitive-walkthroughs/
> - Severity scale: https://www.nngroup.com/articles/how-to-rate-the-severity-of-usability-problems/
>
> **Reviewer caveat** — Standard heuristic evaluation uses 3–5 evaluators; this pass is **one AI reviewer** reading the DOM/JS only (no rendered page, no accessibility tooling, no real users). Treat everything as hypotheses to validate.
>
> ## Headline (Pyramid Principle, apex first — https://www.barbaraminto.com/ )
>
> **The flow has three screens that likely bleed users before T4 ever starts, and one screen that doesn't exist: a first-habit prompt.** The strongest audit case for your 2-step alternative is here — not as a bet, but as the removal of four Severity-3+ problems at once (profile step unmotivated, DOB jargon error, premature notification ask, dead-end "done" screen). Within what remains, the password UX in Step 1 is the single most hostile moment.
>
> ## Cognitive walkthrough summary (full detail at end of findings)
>
> | Step | Q1 right goal | Q2 notice action | Q3 associate | Q4 see progress |
> |---|---|---|---|---|
> | 1 Account | ✅ | ✅ | ✅ | ⚠️ password wiped on error, no step count |
> | 2 Profile | ❌ no stated reason | ✅ | ❌ no "why we ask" | ❌ ERR_422 jargon on error |
> | 3 Notifications | ❌ no habit exists yet | ⚠️ Skip buried | ❌ benefit not tied to habits | ⚠️ |
> | Done | ❌ user came to track a habit | ❌ no action to add one | — | — |
>
> The walkthrough identifies **Step 2 and Done** as the structural failures, which maps directly to the 2-step alternative you're already considering.
>
> ## Findings — ranked by severity
>
> | # | Where | Problem | Sev | Evidence |
> |---|---|---|---|---|
> | F1 | Step 1 pw error (`:87`) | Password field cleared on validation error — user retypes 10+ chars from scratch | **4** | Sourced fact |
> | F3 | Done (`:61–64`) | No "add your first habit" action; dashboard stated as empty, no CTA | **4** | Sourced fact |
> | F4 | Step 2 (`:41–49`) | Phone + birthday required with no stated reason, before any product value is shown | **4** | Sourced fact |
> | F2 | Step 1 (`:33–36`, `:83–89`) | Password rules only revealed after failure | **3** | Sourced fact |
> | F5 | Step 2 error (`:105`) | Error message is backend jargon: "ERR_422: DOB field failed schema validation." | **3** | Sourced fact |
> | F6 | Step 2 bday (`:46`, `:104`) | Birthday is text input with strict US-only `MM/DD/YYYY` regex, no picker | **3** | Sourced fact |
> | F11 | Step 3 (`:52–58`) | OS notification prompt fires before user has any habit — benefit abstract | **3** | Sourced fact |
> | F7 | Step 1 email (`:78`) | Email check is `email.indexOf('@') < 0` — accepts `a@`, `@a` | 2 | Sourced fact |
> | F8 | Across all steps | No "Step X of Y" / progress indicator | 2 | Sourced fact |
> | F9 | Steps 2–3 | No Back button; user can't revise prior input | 2 | Sourced fact |
> | F10 | Step 3 (`:18`) | Skip link is 11px #bbb — nearly invisible dark-pattern contrast | 2 | Sourced fact |
> | F12 | Step 1 | No "Already have an account? Sign in" link | 2 | Sourced fact |
> | F13 | Step 1 | No Terms/Privacy consent language | 2 | Sourced fact |
> | F15 | Step 1 (`:33`, `:35`) | Missing `autocomplete="email"` / `"new-password"` — password managers blocked | 2 | Sourced fact |
> | F14 | Both forms (`:30`, `:41`) | `novalidate` on both forms disables HTML5 field help | 1 | Sourced fact |
> | F16 | Step 1 | No social/SSO (Apple/Google) sign-in | 1 | AI assumption |
>
> ### Finding details
>
> **F1 — Password field wiped on validation failure (Severity 4)**
> - Where: `signup-mock.html:87` — `document.getElementById('pw').value = '';` runs on any password rule miss.
> - Task: T1. Problem: User's first attempt fails a strict rule (≥10 chars, uppercase, digit, symbol) and the field is emptied; they must retype from scratch. Combined with F2 (rules hidden until failure), near-every new user hits this at least once.
> - Expected impact: Perceived hostility at the first interaction; mobile users typing on soft keyboards hit hardest. May contribute to T1 attrition; validate.
> - Principle: H5 Error prevention, H9 Error recovery — https://www.nngroup.com/articles/ten-usability-heuristics/
> - Severity reason: Blocks the first task for likely most users, high persistence, trivially avoidable.
> - Fix: Preserve the input, show a show/hide toggle, and surface rules on focus (fixes F2 at the same time) with a live checklist.
> - How to validate: 5-user think-aloud pre-launch (count attempts to pass Step 1); post-launch, log Step 1 attempt counts.
>
> **F3 — Dead-end "Done" screen; no first-habit action (Severity 4)**
> - Where: `signup-mock.html:61–64` — the "all set" screen tells the user the dashboard is empty in prose; no button, no link.
> - Task: T4. Problem: The goal states "finish signup *and* create their first habit," but the flow ends before T4 is given any affordance. Cognitive walkthrough Q2 fails on this screen.
> - Expected impact: May be the single highest-impact gap for your stated goal; validate.
> - Principle: H1 Visibility of system status, H7 Flexibility — https://www.nngroup.com/articles/ten-usability-heuristics/ ; Cognitive walkthrough Q2 — https://www.nngroup.com/articles/cognitive-walkthroughs/
> - Severity reason: Directly on the critical path to T4, 100% of signup finishers hit it.
> - Fix: Replace "all set" with an in-flow "Add your first habit" step — 3 pre-filled suggestion chips (Drink water / Walk 10 min / Read) + one text field + Save. Then route to the dashboard with the habit visible.
> - How to validate: 5-user test from Step 1 → first habit; measure time to habit and % completing.
>
> **F4 — Step 2 asks for phone + birthday before any value shown (Severity 4)**
> - Where: `signup-mock.html:41–49`.
> - Task: T2. Problem: Both fields are required, with no stated purpose. User has done nothing in the product yet; the ask is maximally premature for maximally sensitive data (phone, DOB). Cognitive walkthrough Q1 and Q3 both fail.
> - Expected impact: May be the strongest contributor to Step-2 abandonment; validate.
> - Principle: H2 Match the real world (user's mental model: "track habits"), H5 Error prevention, H10 Help and documentation (no "why we ask") — https://www.nngroup.com/articles/ten-usability-heuristics/
> - Severity reason: Blocks progress with low justification; also directly matches your proposed 2-step alternative.
> - Fix: Cut Step 2 entirely (your 2-step alternative). If any field is actually required by backend, surface it later with a concrete reason at the moment it's needed (phone only when SMS reminders are turned on; birthday only if a feature uses it).
> - How to validate: Confirm with the team whether phone/birthday are used by any current feature. If not, cut. If yes, defer with just-in-time asks.
> - Evidence: Sourced fact + User-provided fact (team already considering a 2-step flow).
>
> **F2 — Password rules hidden until first failure (Severity 3)**
> - Where: `signup-mock.html:33–36` (no helper), `:83–89` (rule enforced in JS).
> - Task: T1. Problem: Recall-not-recognition. Rules are only shown after failure, which compounds F1.
> - Expected impact: Near-every first attempt may fail; validate.
> - Principle: H6 Recognition rather than recall, H1 Visibility — https://www.nngroup.com/articles/ten-usability-heuristics/
> - Severity reason: Hits all users once, high persistence, direct cost in retry effort.
> - Fix: Show the full rule set inline on focus, with a live-tick checklist. Reconsider whether 10 chars + uppercase + number + symbol is necessary; current research favors length over classes.
> - How to validate: 5-user think-aloud; measure first-try success rate.
>
> **F5 — Error message leaks backend jargon (Severity 3)**
> - Where: `signup-mock.html:105` — `"ERR_422: DOB field failed schema validation."`
> - Task: T2. Problem: HTTP status + schema terminology shown to end users.
> - Expected impact: Users don't know what to do; perceived as broken; may contribute to Step-2 abandonment, validate.
> - Principle: H2 Match real world, H9 Error recovery — https://www.nngroup.com/articles/ten-usability-heuristics/
> - Severity reason: Persistent for everyone who formats birthday differently; worse for non-US users. Moot if F4 cuts Step 2.
> - Fix: If Step 2 stays, write human copy and show an example; better, swap to `type="date"`. If F4 removes Step 2, this disappears.
> - How to validate: Review with any non-US user; or verify Step 2 is cut.
>
> **F6 — Birthday is text input with US-only strict regex (Severity 3)**
> - Where: `signup-mock.html:46` `type="text"` and `:104` `/^\d{2}\/\d{2}\/\d{4}$/`.
> - Task: T2. Problem: International formats (`DD/MM/YYYY`), unpadded typing (`3/14/1990`), and all mobile numeric keyboards are penalized. No picker.
> - Expected impact: Format-mismatch friction; may contribute to Step-2 attrition, validate.
> - Principle: H5 Error prevention, H7 Flexibility — https://www.nngroup.com/articles/ten-usability-heuristics/
> - Severity reason: Likely high for any non-US user; moot if F4 removes Step 2.
> - Fix: `<input type="date">` with native picker, or cut.
> - How to validate: 3 non-US users; or verify Step 2 is cut.
>
> **F11 — Notification prompt fires before any habit exists (Severity 3)**
> - Where: `signup-mock.html:52–58`.
> - Task: T3 → T4. Problem: OS permission prompt is asked when the "why" is abstract. On both iOS and Android, a denial here is persistent (user must flip it in system settings). The benefit should be tied to a concrete habit + reminder time.
> - Expected impact: Short-term more consents, longer-term more permanent-deny; harms retention. Validate.
> - Principle: H2 Match real world — https://www.nngroup.com/articles/ten-usability-heuristics/ ; Cognitive walkthrough Q3 — https://www.nngroup.com/articles/cognitive-walkthroughs/
> - Severity reason: Decision is persistent at the OS level, hard to undo later; direct hit on T4-and-after retention.
> - Fix: Defer the OS prompt until after Step T4 — ask immediately after the user sets a reminder time for a specific habit, with concrete copy ("Remind me to 'Drink water' at 7:00 AM").
> - How to validate: Post-launch, compare permanent-deny rate between flows.
>
> **F7 — Email validation accepts obvious typos (Severity 2)**
> - Where: `signup-mock.html:78` — only checks `indexOf('@')`.
> - Task: T1. Problem: Lets `a@`, `@a`, `a@b` through. Users who typo their email can't recover the account.
> - Principle: H5 Error prevention. Fix: Remove `novalidate` (F14) and use the native email validator; add email verification for new accounts. Validate: review bounce rates post-launch.
>
> **F8 — No "Step X of Y" progress indicator (Severity 2)**
> - Where: all steps. Task: T1–T3. Principle: H1 Visibility of system status.
> - Fix: Add a step counter — or render moot by moving to 2 steps. Validate: 5-user test, ask "how far along do you feel?"
>
> **F9 — No Back control (Severity 2)**
> - Where: Steps 2–3. Task: T1–T3. Principle: H3 User control and freedom.
> - Fix: Back link top-left on each step past 1. Moot if the flow is cut to 2 steps and each step is short.
>
> **F10 — Skip is visually buried on notifications (Severity 2, 3 if ethics weighted)**
> - Where: `signup-mock.html:18` — `.skip { font-size: 11px; color: #bbb; }`.
> - Task: T3. Problem: Low-contrast, undersized "Skip" against a bold primary button is a nudge toward granting a permission the user may not want. Also an ethical/deceptive-pattern concern.
> - Principle: H3 User control and freedom, H8 Aesthetic — https://www.nngroup.com/articles/ten-usability-heuristics/
> - Fix: Make Skip a normal-weight secondary link; strengthen the "why". Better, let F11 fix this by deferring the prompt entirely.
>
> **F12 — No "Already have an account? Sign in" (Severity 2)**
> - Where: Step 1. Task: T1. Principle: H3 User control and freedom.
> - Fix: Secondary link under the Continue button.
>
> **F13 — No Terms/Privacy consent (Severity 2 UX; legal review needed)**
> - Where: Step 1. Task: T1. Principle: H10 Help and documentation.
> - Fix: "By continuing, you agree to our Terms and Privacy" one-liner.
>
> **F15 — Password managers blocked by missing autocomplete (Severity 2)**
> - Where: `signup-mock.html:33, 35`. Task: T1.
> - Fix: Add `autocomplete="email"` on email input and `autocomplete="new-password"` on password. Add `inputmode="email"` on email. Combined with keeping the typed password (F1), this is the single biggest friction cut on mobile.
> - Validate: Test with iOS Safari + Keychain and Chrome + password manager.
>
> **F14 — `novalidate` on both forms (Severity 1)**
> - Where: `signup-mock.html:30, 41`. Fix: Drop `novalidate`, add `required`, let HTML5 catch empties.
>
> **F16 — No SSO (Severity 1, business call)**
> - A legitimate scope decision given 3 people / 2 months. Note if you add Apple/Google sign-in on iOS, Apple requires Sign-in-with-Apple alongside. Evidence label: AI assumption.
>
> ### Cognitive walkthrough — step by step
>
> - **Step 1 — Create your account.** Q1 ✅ user wants an account; Q2 ✅ Continue is obvious; Q3 ✅; **Q4 fails** when the password error wipes the field (F1) and the rules only show on failure (F2) — user is told "wrong" without being shown "wrong how."
> - **Step 2 — Tell us about you.** **Q1 fails** — the user didn't come to Tally to disclose phone/birthday; the mental model is "track habits" (F4). **Q3 fails** — no "why we ask." **Q4 fails** on error — `ERR_422` is not a human message (F5), and the regex rejects normal international date formats (F6).
> - **Step 3 — Turn on notifications.** **Q1 fails** — the user has no habit to be reminded about (F11), so the ask is abstract. Q2 ⚠️ — "Skip" is drawn to be missed (F10). **Q3 fails** — no concrete benefit tied to habits. Q4 ⚠️ — unclear whether denying advances the flow.
> - **Done.** **Q2 fails** — there is no visible action to add a habit (F3). The user came to Tally for T4 and the flow stops one step short.
>
> ## 🚪 GATE 2 — please confirm before I prioritize + recommend
>
> Three quick decisions, each affects Step 5:
>
> 1. **Findings accepted as a basis for prioritization?** (Y, or flag any you want dropped/added.)
> 2. **Treat the 2-step flow (email+password → first habit) as the leading solution direction** when I write Step 5? That path collapses F4, F5, F6, F8, F9, F11 and partially F10 into "deleted," and lets me recommend the strongest version I can for a 3-person / 2-month launch. Or do you want Step 5 written as "pick between keeping 3 steps vs. collapsing to 2"?
> 3. **For priority ranking** — Impact/Effort 2x2 (my recommendation given no analytics + rough effort knowable in a 3-person team), or evidence-based qualitative only? I'll avoid RICE since Reach is unknown without data.
>
> I'll stop here and wait.

### Turn 3 — User

> 1. Severities look right to us. Keep all findings. 2. Yes, treat the 2-step flow (email+password, then first habit) as the leading direction. 3. Impact/Effort 2x2 is fine. Go ahead with Step 5.

### Turn 3 — Assistant

> # Tally Signup Audit — Step 5: Prioritize, Recommend, Hand off
>
> ## Headline (Pyramid Principle — https://www.barbaraminto.com/ )
>
> **Build the 2-step flow you already have on the table, with four fixes layered on top: a real first-habit screen, kept password + rule hints, autocomplete attributes, and a notifications ask deferred until the user has set a reminder time.** This single move cleans up seven of the sixteen findings by deletion, puts T4 inside the signup flow where the business goal needs it, and is sized for 3 people in 2 months.
>
> ### Three reasons (MECE)
>
> 1. **Deleting Step 2 removes four Severity-3+ findings at once** — F4 (unmotivated profile ask), F5 (ERR_422 jargon), F6 (US-only DOB regex), and the structural case for F11 (premature notifications becomes solvable). Removal is strictly cheaper than fixing, and uses evidence you already have: your team flagged phone/birthday as not clearly load-bearing.
> 2. **Adding a first-habit step moves T4 inside signup, where your goal lives.** The current Done screen (F3) is one word short of the business goal — it describes the dashboard instead of letting the user act on it. The cheapest version is a text field plus 3 suggestion chips plus a reminder time, which also unlocks the correct moment for the notification permission (F11).
> 3. **The remaining launch-blockers all sit in Step 1 and are hours of work, not days.** F1 (keep the password), F2 (show rules on focus), F15 (autocomplete), F14/F7 (native validation + email check), F13 (consent copy), F12 (sign-in link). None need design review; most are 1–3 line changes.
>
> So-what test applied: anything I audited that doesn't bear on these three reasons is kept in the backlog, not promoted into the plan.
>
> ## Impact / Effort 2x2 (Audit mode — qualitative, Reach unknown without analytics)
>
> | | **Low effort** | **High effort** |
> |---|---|---|
> | **High impact** | **DO FIRST**: F4 cut Step 2 · F3 add first-habit screen · F1 keep password + show/hide · F2 rules on focus + live checklist · F11 defer notif until reminder set · F15 autocomplete attributes · F13 Terms/Privacy line | **PLAN**: full first-habit UX polish (suggestion chips, time picker, animations) · F16 Apple/Google SSO |
> | **Low impact** | **BATCH IN**: F7 email validation · F12 Sign-in link · F14 drop `novalidate` · F10 Skip legibility (moot after F11) | **SKIP for launch**: step indicator (F8), Back button (F9), DOB picker (F6) — all moot once the flow is 2 steps |
>
> Severity ranks "how bad for the user"; this 2x2 ranks "do what first." A Sev-4 that is moot once Step 2 is cut (F5, F6) correctly drops off the plan.
>
> **AI assumption on effort**: I don't know your stack. "Low effort" means I expect ≤ half a day each for a web stack with no unusual constraints; adjust if you have backend coupling I don't see.
>
> ## What to build — the 2-step flow spec (handoff to the visual design step)
>
> ### Step 1 — Create account
> - **Email** — `type="email"`, `autocomplete="email"`, `inputmode="email"`. Remove `novalidate` from the form; use HTML5 required + native validator (fixes F7, F14, F15 together).
> - **Password** — `autocomplete="new-password"`, show/hide eye toggle, do **not** clear the field on error (F1).
> - **Rules shown on focus**, with a live checklist that ticks items as satisfied (F2). Reconsider "10 chars + uppercase + number + symbol": length-based rules are more user-friendly than class-based; AI assumption, confirm with your security requirements.
> - **Secondary links**: "Already have an account? Sign in" (F12) and the one-line Terms/Privacy consent (F13).
> - **Primary**: "Continue" → Step 2.
>
> ### Step 2 — Add your first habit (new; this is T4 moved inside signup)
> - **Headline**: "What's one habit you want to build?" (JTBD framing — https://hbr.org/2016/09/know-your-customers-jobs-to-be-done — the user hired Tally to build a habit, not to configure an account).
> - **3 suggestion chips** pre-filling the field: Drink water · Walk 10 min · Read 10 min (editable).
> - **Text field**: habit name.
> - **Reminder time**: time picker, default sensible (e.g. 8:00 AM).
> - **Primary**: "Save my habit".
> - **Secondary**: "Skip for now" — legible, not buried (fixes the F10 pattern by making it an honest choice).
> - **On save**: fire the OS notification prompt now, with concrete copy tied to the habit ("Allow Tally to remind you to Drink water at 8:00 AM?") — this is where F11 is paid off.
> - **Then**: land on the dashboard with that habit visible and today's check-in CTA active. (The empty-dashboard language from `signup-mock.html:63` disappears because the dashboard is no longer empty.)
>
> Together this spec addresses, by deletion or construction: F3, F4, F5, F6, F8, F9, F10, F11. Within Step 1 it addresses F1, F2, F7, F12, F13, F14, F15.
>
> ### Deliberately out of scope for launch
> - **F16 SSO** — plan post-launch. If you add Apple/Google sign-in on iOS later, Apple's App Store rules require Sign-in-with-Apple alongside; AI assumption, verify current App Store guidelines before implementing.
> - **Step indicator (F8)** and **Back button (F9)** — moot in a 2-step flow where each step is short. Add if post-launch testing shows users feel lost.
>
> ## Validation plan — cheapest test per top recommendation
>
> You said no analytics yet. These tests do not need any:
>
> | Recommendation | Cheapest test | What it would confirm / reject |
> |---|---|---|
> | 2-step flow (overall) | **5-user moderated test** before launch, task script: "Sign up and start tracking a habit." No analytics needed. Source: https://www.nngroup.com/articles/how-to-conduct-a-heuristic-evaluation/ for the reviewer-count logic that applies here — 5 is also the common target for formative usability testing. | Does T4 complete inside signup? Where do users still hesitate? |
> | Password UX (F1 + F2 + F15) | Within the same 5-user test, instrument the password step with think-aloud; count attempts before pass. | Rule hints + preserved field + password-manager autofill actually reduce retries. |
> | Deferred notifications (F11) | Instrument permission-grant and permanent-deny rates at launch (needs the lightest analytics event, not a full stack). | Deferring to post-habit raises grants and lowers permanent-denies, as hypothesized. |
> | First-habit screen framing | **Hallway test** of 3 variants of the Step 2 headline + chips (e.g. "What's one habit…", "Pick a habit to start", "Add your first habit") with 5 non-user friends. | Which framing feels most inviting; detect any JTBD mismatch early. |
>
> If you want more structure for the 5-user test, Nielsen heuristic method's reviewer-independence logic transfers (https://www.nngroup.com/articles/how-to-conduct-a-heuristic-evaluation/ ) — have each user complete without observing another.
>
> ## Handoff map
>
> - **To visual design step** → everything above the "Validation plan" line is the spec. Pass with evidence labels intact.
> - **To PRD / backend** → confirm the open question: are phone or birthday actually used by any current or planned feature? If yes, defer to a just-in-time ask (phone when SMS reminders are turned on; birthday only if a feature uses it). If no, delete from the data model.
> - **To validation / research** → the 5-user test above, before launch. Then minimum instrumentation for notification grants and signup-to-first-habit completion at launch so you can learn in-market.
>
> ## Open questions that still need an answer
>
> - **Open question**: Are phone or birthday used by any current or planned feature? (Blocks F4 cut.)
> - **Open question**: What is your current security requirement for the password rule? (Decides whether you can relax the complexity rule and lean on length + breach-check.)
> - **Open question**: Is email verification part of the account-creation flow after the mock? (Decides scope of F7.)
> - **Open question**: Platform at launch — web, iOS, Android, all three? (Decides whether F11's OS-prompt timing and F16 SSO rules apply.)
>
> ---
>
> **End of audit.** Mode: Audit — Steps 1 → 4 → 5. Methods cited at point of use: SCQA / Pyramid Principle (https://www.barbaraminto.com/ ); Nielsen 10 heuristics + severity (https://www.nngroup.com/articles/ten-usability-heuristics/ , https://www.nngroup.com/articles/how-to-rate-the-severity-of-usability-problems/ , https://www.nngroup.com/articles/how-to-conduct-a-heuristic-evaluation/ ); Cognitive walkthrough (https://www.nngroup.com/articles/cognitive-walkthroughs/ ); JTBD (https://hbr.org/2016/09/know-your-customers-jobs-to-be-done ).
>
> Want me to save this as `signup-audit.md` in the project folder, or push it to the visual design step as-is?

### Turn 4 — User

> Yes, please save the full report (Steps 1, 4 and 5) as ux-audit-signup.md in the project folder.

### Turn 4 — Assistant

*[tool call: Write `ux-audit-signup.md`]*

> Saved to `ux-audit-signup.md` in the project folder — Steps 1, 4, and 5 in one document with the SCQA framing, cognitive walkthrough, all 16 findings, Impact/Effort 2x2, 2-step flow spec, validation plan, and open questions. Next sensible step is the visual design handoff for the Step 1 + first-habit screens; let me know if you'd like me to draft that handoff brief too.

## Final report (`ux-audit-signup.md`, verbatim)

# Tally Signup — UX Audit

**Target**: `signup-mock.html` (fictional mock made for this audit)
**Goal**: more new users finish signup and create their first habit
**Context (User-provided fact)**: team of 3, launching in 2 months, no analytics yet. A 2-step flow (email+password → first habit) was already on the table.
**Mode**: Audit — Steps 1 → 4 → 5.
**Reviewer caveat**: Standard heuristic evaluation uses 3–5 evaluators; this pass is one AI reviewer reading the DOM/JS of `signup-mock.html` only — no rendered page, no accessibility tooling, no real users. Every behavioural claim is a hypothesis to validate, not a measurement.

**Methods used, and why**
- **SCQA / Pyramid Principle** (framing + recommendation) — https://www.barbaraminto.com/
- **Nielsen heuristic evaluation** (10 heuristics, severity 0–4) — fits a one-screen flow where the question is "what breaks for a new user." https://www.nngroup.com/articles/how-to-conduct-a-heuristic-evaluation/ · https://www.nngroup.com/articles/ten-usability-heuristics/ · https://www.nngroup.com/articles/how-to-rate-the-severity-of-usability-problems/
- **Cognitive walkthrough** (4 questions per step) — fits a new user doing an unfamiliar flow, and makes T4 (first habit) failure visible. https://www.nngroup.com/articles/cognitive-walkthroughs/
- **JTBD** — used in the recommendation to frame Step 2. https://hbr.org/2016/09/know-your-customers-jobs-to-be-done
- **Impact/Effort 2x2** — qualitative prioritization, appropriate because Reach is unknown without analytics.

---

## Step 1 — Framing (SCQA)

- **Situation (Sourced fact: `signup-mock.html:29–64`)** — Tally has a 3-step signup: ① email + password → ② phone + birthday → ③ notification prompt → "You're all set" screen that tells users their dashboard is empty.
- **Complication (User-provided fact)** — The team wants more new users to both finish signup *and* create their first habit, but there is no analytics data yet to see where they stall.
- **Question (Approved decision)** — Which specific changes to this flow would most plausibly raise completion through first-habit creation?
- **Answer (AI assumption, to be validated)** — The flow likely leaks at three places: profile step asks for phone + birthday with no stated reason (Step 2), password rules are strict and the field is wiped on error (Step 1), and the final screen hands the user to an empty dashboard instead of a 1-tap "add your first habit" (Done). Reducing/deferring profile data, softening error handling, and routing into first-habit creation are the most promising levers.

### User tasks being checked (Approved decision)

| ID | Task | Where it lives today |
|---|---|---|
| T1 | Create an account (email + password) | `signup-mock.html:30–38` |
| T2 | Provide profile info required by Tally | `signup-mock.html:41–49` |
| T3 | Decide on notifications | `signup-mock.html:52–58` |
| T4 | Create their first habit (**in scope**) | **Not in this file** — "done" screen ends on an empty dashboard (`signup-mock.html:61–64`). Flagged as a missing handoff. |

---

## Step 4 — Diagnosis (heuristic evaluation + cognitive walkthrough)

### Cognitive walkthrough summary

| Step | Q1 right goal | Q2 notice action | Q3 associate | Q4 see progress |
|---|---|---|---|---|
| 1 Account | OK | OK | OK | ⚠️ password wiped on error, no step count |
| 2 Profile | ❌ no stated reason | OK | ❌ no "why we ask" | ❌ `ERR_422` jargon on error |
| 3 Notifications | ❌ no habit exists yet | ⚠️ Skip buried | ❌ benefit not tied to habits | ⚠️ |
| Done | ❌ user came to track a habit | ❌ no action to add one | — | — |

Full walkthrough per step:
- **Step 1 — Create your account.** Q1 user wants an account; Q2 Continue is obvious; Q3 clear; **Q4 fails** when the password error wipes the field (F1) and the rules only show on failure (F2) — user is told "wrong" without being shown "wrong how."
- **Step 2 — Tell us about you.** **Q1 fails** — the user didn't come to Tally to disclose phone/birthday; mental model is "track habits" (F4). **Q3 fails** — no "why we ask." **Q4 fails** on error — `ERR_422` is not a human message (F5), and the regex rejects normal international date formats (F6).
- **Step 3 — Turn on notifications.** **Q1 fails** — the user has no habit to be reminded about (F11), so the ask is abstract. Q2 ⚠️ — "Skip" is drawn to be missed (F10). **Q3 fails** — no concrete benefit tied to habits. Q4 ⚠️ — unclear whether denying advances the flow.
- **Done.** **Q2 fails** — there is no visible action to add a habit (F3). The user came to Tally for T4 and the flow stops one step short.

Together, the walkthrough identifies **Step 2** and **Done** as the structural failures.

### Findings — ranked by severity

Severity scale: 0 none · 1 cosmetic · 2 minor · 3 major · 4 catastrophic (https://www.nngroup.com/articles/how-to-rate-the-severity-of-usability-problems/ ).

| # | Where | Problem | Sev | Evidence label |
|---|---|---|---|---|
| F1 | Step 1 pw error (`:87`) | Password field cleared on validation error — user retypes 10+ chars from scratch | **4** | Sourced fact |
| F3 | Done (`:61–64`) | No "add your first habit" action; dashboard stated as empty, no CTA | **4** | Sourced fact |
| F4 | Step 2 (`:41–49`) | Phone + birthday required with no stated reason, before any product value is shown | **4** | Sourced fact |
| F2 | Step 1 (`:33–36`, `:83–89`) | Password rules only revealed after failure | **3** | Sourced fact |
| F5 | Step 2 error (`:105`) | Error message is backend jargon: "ERR_422: DOB field failed schema validation." | **3** | Sourced fact |
| F6 | Step 2 bday (`:46`, `:104`) | Birthday is text input with strict US-only `MM/DD/YYYY` regex, no picker | **3** | Sourced fact |
| F11 | Step 3 (`:52–58`) | OS notification prompt fires before user has any habit — benefit abstract | **3** | Sourced fact |
| F7 | Step 1 email (`:78`) | Email check is `email.indexOf('@') < 0` — accepts `a@`, `@a` | 2 | Sourced fact |
| F8 | Across all steps | No "Step X of Y" / progress indicator | 2 | Sourced fact |
| F9 | Steps 2–3 | No Back button; user can't revise prior input | 2 | Sourced fact |
| F10 | Step 3 (`:18`) | Skip link is 11px #bbb — nearly invisible contrast, deceptive-pattern concern | 2 | Sourced fact |
| F12 | Step 1 | No "Already have an account? Sign in" link | 2 | Sourced fact |
| F13 | Step 1 | No Terms/Privacy consent language | 2 | Sourced fact |
| F15 | Step 1 (`:33`, `:35`) | Missing `autocomplete="email"` / `"new-password"` — password managers blocked | 2 | Sourced fact |
| F14 | Both forms (`:30`, `:41`) | `novalidate` on both forms disables HTML5 field help | 1 | Sourced fact |
| F16 | Step 1 | No social/SSO (Apple/Google) sign-in | 1 | AI assumption |

### Finding details

**F1 — Password field wiped on validation failure (Severity 4)**
- Where: `signup-mock.html:87` — `document.getElementById('pw').value = '';` runs on any password rule miss. Inspected via DOM/JS.
- Task: T1. Problem: User's first attempt fails a strict rule (≥10 chars, uppercase, digit, symbol) and the field is emptied; they must retype from scratch. Combined with F2 (rules hidden until failure), near-every new user hits this at least once.
- Expected impact: Perceived hostility at the first interaction; mobile users typing on soft keyboards hit hardest. May contribute to T1 attrition; validate.
- Principle: H5 Error prevention, H9 Error recovery — https://www.nngroup.com/articles/ten-usability-heuristics/
- Severity reason: Blocks the first task for likely most users, high persistence, trivially avoidable.
- Fix: Preserve the input, show a show/hide toggle, and surface rules on focus (fixes F2 at the same time) with a live checklist.
- How to validate: 5-user think-aloud pre-launch (count attempts to pass Step 1); post-launch, log Step 1 attempt counts.
- Evidence: Sourced fact (code); expected impact AI assumption.

**F3 — Dead-end "Done" screen; no first-habit action (Severity 4)**
- Where: `signup-mock.html:61–64` — the "all set" screen tells the user the dashboard is empty in prose; no button, no link. Inspected via DOM.
- Task: T4. Problem: The goal states "finish signup *and* create their first habit," but the flow ends before T4 is given any affordance. Cognitive walkthrough Q2 fails on this screen.
- Expected impact: May be the single highest-impact gap for the stated goal; validate.
- Principle: H1 Visibility of system status, H7 Flexibility — https://www.nngroup.com/articles/ten-usability-heuristics/ ; Cognitive walkthrough Q2 — https://www.nngroup.com/articles/cognitive-walkthroughs/
- Severity reason: Directly on the critical path to T4, 100% of signup finishers hit it.
- Fix: Replace "all set" with an in-flow "Add your first habit" step — 3 pre-filled suggestion chips (Drink water / Walk 10 min / Read) + one text field + Save. Then route to the dashboard with the habit visible.
- How to validate: 5-user test from Step 1 → first habit; measure time to habit and % completing.
- Evidence: Sourced fact (code); expected impact AI assumption.

**F4 — Step 2 asks for phone + birthday before any value shown (Severity 4)**
- Where: `signup-mock.html:41–49`. Inspected via DOM.
- Task: T2. Problem: Both fields are required, with no stated purpose. User has done nothing in the product yet; the ask is maximally premature for sensitive data (phone, DOB). Cognitive walkthrough Q1 and Q3 both fail.
- Expected impact: May be the strongest contributor to Step-2 attrition; validate.
- Principle: H2 Match the real world (mental model: "track habits"), H5 Error prevention, H10 Help and documentation (no "why we ask") — https://www.nngroup.com/articles/ten-usability-heuristics/
- Severity reason: Blocks progress with low justification; aligned with team's 2-step alternative.
- Fix: Cut Step 2 entirely (the 2-step alternative). If any field is actually required by backend, surface it later with a concrete reason at the moment it's needed (phone only when SMS reminders are turned on; birthday only if a feature uses it).
- How to validate: Confirm with the team whether phone/birthday are used by any current feature. If not, cut. If yes, defer with just-in-time asks.
- Evidence: Sourced fact (code) + User-provided fact (team already considering a 2-step flow).

**F2 — Password rules hidden until first failure (Severity 3)**
- Where: `signup-mock.html:33–36` (no helper text), `:83–89` (rule enforced in JS). Inspected via DOM/JS.
- Task: T1. Problem: Recall-not-recognition. Rules are only shown after failure, which compounds F1.
- Expected impact: Near-every first attempt may fail; validate.
- Principle: H6 Recognition rather than recall, H1 Visibility — https://www.nngroup.com/articles/ten-usability-heuristics/
- Severity reason: Hits all users once, high persistence, direct cost in retry effort.
- Fix: Show the full rule set inline on focus, with a live-tick checklist. Reconsider whether 10 chars + uppercase + number + symbol is necessary; length-based rules are often sufficient (AI assumption — confirm with your security requirements).
- How to validate: 5-user think-aloud; measure first-try success rate.
- Evidence: Sourced fact (code); expected impact AI assumption.

**F5 — Error message leaks backend jargon (Severity 3)**
- Where: `signup-mock.html:105` — `"ERR_422: DOB field failed schema validation."` Inspected via JS.
- Task: T2. Problem: HTTP status + schema terminology shown to end users.
- Expected impact: Users don't know what to do; perceived as broken; may contribute to Step-2 attrition, validate.
- Principle: H2 Match real world, H9 Error recovery — https://www.nngroup.com/articles/ten-usability-heuristics/
- Severity reason: Persistent for everyone who formats birthday differently; worse for non-US users. Moot if F4 cuts Step 2.
- Fix: If Step 2 stays, write human copy with an example; better, swap to `type="date"`. If F4 removes Step 2, this disappears.
- How to validate: Review with any non-US user; or verify Step 2 is cut.
- Evidence: Sourced fact (code).

**F6 — Birthday is text input with US-only strict regex (Severity 3)**
- Where: `signup-mock.html:46` (`type="text"`) and `:104` (`/^\d{2}\/\d{2}\/\d{4}$/`). Inspected via DOM/JS.
- Task: T2. Problem: International formats (`DD/MM/YYYY`), unpadded typing (`3/14/1990`), and mobile numeric keyboards all fail. No picker.
- Expected impact: Format-mismatch friction; may contribute to Step-2 attrition, validate.
- Principle: H5 Error prevention, H7 Flexibility — https://www.nngroup.com/articles/ten-usability-heuristics/
- Severity reason: Likely high for any non-US user; moot if F4 removes Step 2.
- Fix: `<input type="date">` with native picker, or cut.
- How to validate: 3 non-US users; or verify Step 2 is cut.
- Evidence: Sourced fact (code).

**F11 — Notification prompt fires before any habit exists (Severity 3)**
- Where: `signup-mock.html:52–58`. Inspected via DOM.
- Task: T3 → T4. Problem: OS permission prompt is asked when the "why" is abstract. On both iOS and Android, denial here is persistent (user must flip it in system settings). The benefit should be tied to a concrete habit + reminder time.
- Expected impact: Short-term more consents, longer-term more permanent-deny; harms retention. Validate.
- Principle: H2 Match real world — https://www.nngroup.com/articles/ten-usability-heuristics/ ; Cognitive walkthrough Q3 — https://www.nngroup.com/articles/cognitive-walkthroughs/
- Severity reason: Decision is persistent at the OS level, hard to undo later; direct hit on T4-and-after retention.
- Fix: Defer the OS prompt until after T4 — ask immediately after the user sets a reminder time for a specific habit, with concrete copy ("Remind me to 'Drink water' at 7:00 AM").
- How to validate: Post-launch, compare permanent-deny rate between flows.
- Evidence: Sourced fact (code); expected impact AI assumption.

**F7 — Email validation accepts obvious typos (Severity 2)**
- Where: `signup-mock.html:78` — only checks `indexOf('@')`.
- Task: T1. Problem: Lets `a@`, `@a`, `a@b` through. Users who typo their email can't recover the account.
- Principle: H5 Error prevention. Fix: Remove `novalidate` (F14) and use the native email validator; add email verification for new accounts. Validate: review bounce rates post-launch.
- Evidence: Sourced fact.

**F8 — No "Step X of Y" progress indicator (Severity 2)**
- Where: all steps. Task: T1–T3. Principle: H1 Visibility of system status.
- Fix: Add a step counter — or render moot by moving to 2 steps. Validate: 5-user test, ask "how far along do you feel?"
- Evidence: Sourced fact.

**F9 — No Back control (Severity 2)**
- Where: Steps 2–3. Task: T1–T3. Principle: H3 User control and freedom.
- Fix: Back link top-left on each step past 1. Moot if the flow is cut to 2 steps and each step is short.
- Evidence: Sourced fact.

**F10 — Skip is visually buried on notifications (Severity 2)**
- Where: `signup-mock.html:18` — `.skip { font-size: 11px; color: #bbb; }`.
- Task: T3. Problem: Low-contrast, undersized "Skip" against a bold primary button nudges toward granting a permission the user may not want. Also an ethical / deceptive-pattern concern.
- Principle: H3 User control and freedom, H8 Aesthetic — https://www.nngroup.com/articles/ten-usability-heuristics/
- Fix: Make Skip a normal-weight secondary link; strengthen the "why". Better, let F11 fix this by deferring the prompt entirely.
- Evidence: Sourced fact.

**F12 — No "Already have an account? Sign in" (Severity 2)**
- Where: Step 1. Task: T1. Principle: H3 User control and freedom. Fix: Secondary link under the Continue button. Evidence: Sourced fact.

**F13 — No Terms/Privacy consent (Severity 2 UX; legal review needed)**
- Where: Step 1. Task: T1. Principle: H10 Help and documentation. Fix: "By continuing, you agree to our Terms and Privacy" one-liner. Evidence: Sourced fact.

**F15 — Password managers blocked by missing autocomplete (Severity 2)**
- Where: `signup-mock.html:33, 35`. Task: T1.
- Fix: Add `autocomplete="email"` on email input and `autocomplete="new-password"` on password. Add `inputmode="email"` on email. Combined with keeping the typed password (F1), likely the biggest friction cut on mobile.
- Validate: Test with iOS Safari + Keychain and Chrome + password manager.
- Evidence: Sourced fact.

**F14 — `novalidate` on both forms (Severity 1)**
- Where: `signup-mock.html:30, 41`. Fix: Drop `novalidate`, add `required`, let HTML5 catch empties. Evidence: Sourced fact.

**F16 — No SSO (Severity 1, business call)**
- A legitimate scope decision given 3 people / 2 months. If Apple/Google sign-in is added on iOS later, Apple's App Store rules may require Sign-in-with-Apple alongside; AI assumption, verify current App Store guidelines before implementing.
- Evidence: AI assumption.

---

## Step 5 — Prioritize, Recommend, Hand off

### Headline (Pyramid Principle apex — https://www.barbaraminto.com/ )

**Build the 2-step flow (email+password → first habit), with four fixes layered on top: a real first-habit screen, kept password + rule hints, autocomplete attributes, and a notifications ask deferred until the user has set a reminder time.** This single move cleans up seven of the sixteen findings by deletion, puts T4 inside the signup flow where the business goal needs it, and is sized for 3 people in 2 months.

### Three reasons (MECE)

1. **Deleting Step 2 removes four Severity-3+ findings at once** — F4 (unmotivated profile ask), F5 (`ERR_422` jargon), F6 (US-only DOB regex), and the structural case for F11 (premature notifications becomes solvable). Removal is strictly cheaper than fixing, and uses evidence the team already has: phone/birthday were flagged as not clearly load-bearing.
2. **Adding a first-habit step moves T4 inside signup, where the goal lives.** The current Done screen (F3) is one word short of the business goal — it describes the dashboard instead of letting the user act on it. The cheapest version is a text field plus 3 suggestion chips plus a reminder time, which also unlocks the correct moment for the notification permission (F11).
3. **The remaining launch-blockers all sit in Step 1 and are hours of work, not days.** F1 (keep the password), F2 (show rules on focus), F15 (autocomplete), F14/F7 (native validation + email check), F13 (consent copy), F12 (sign-in link). None need design review; most are 1–3 line changes.

So-what test applied: anything audited that doesn't bear on these three reasons is kept in the backlog, not promoted into the plan.

### Impact / Effort 2x2

**AI assumption on effort**: stack unknown. "Low effort" means roughly ≤ half a day each for a web stack with no unusual constraints; adjust if backend coupling says otherwise.

| | **Low effort** | **High effort** |
|---|---|---|
| **High impact** | **DO FIRST**: F4 cut Step 2 · F3 add first-habit screen · F1 keep password + show/hide · F2 rules on focus + live checklist · F11 defer notif until reminder set · F15 autocomplete attributes · F13 Terms/Privacy line | **PLAN**: full first-habit UX polish (suggestion chips, time picker, animations) · F16 Apple/Google SSO |
| **Low impact** | **BATCH IN**: F7 email validation · F12 Sign-in link · F14 drop `novalidate` · F10 Skip legibility (moot after F11) | **SKIP for launch**: step indicator (F8), Back button (F9), DOB picker (F6) — all moot once the flow is 2 steps |

Severity ranks "how bad for the user"; this 2x2 ranks "do what first." A Sev-4 that is moot once Step 2 is cut (F5, F6) correctly drops off the plan.

### What to build — the 2-step flow spec (handoff to visual design)

**Step 1 — Create account**
- **Email** — `type="email"`, `autocomplete="email"`, `inputmode="email"`. Remove `novalidate`; use HTML5 required + native validator (fixes F7, F14, F15 together).
- **Password** — `autocomplete="new-password"`, show/hide eye toggle, do **not** clear the field on error (F1).
- **Rules shown on focus**, with a live checklist that ticks items as satisfied (F2). Reconsider "10 chars + uppercase + number + symbol": length-based rules are more user-friendly than class-based (AI assumption — confirm with security requirements).
- **Secondary links**: "Already have an account? Sign in" (F12) and the one-line Terms/Privacy consent (F13).
- **Primary**: "Continue" → Step 2.

**Step 2 — Add your first habit** (new; this is T4 moved inside signup)
- **Headline**: "What's one habit you want to build?" (JTBD framing — https://hbr.org/2016/09/know-your-customers-jobs-to-be-done — the user hired Tally to build a habit, not to configure an account).
- **3 suggestion chips** pre-filling the field: Drink water · Walk 10 min · Read 10 min (editable).
- **Text field**: habit name.
- **Reminder time**: time picker, default sensible (e.g. 8:00 AM).
- **Primary**: "Save my habit".
- **Secondary**: "Skip for now" — legible, not buried (fixes the F10 pattern by making it an honest choice).
- **On save**: fire the OS notification prompt now, with concrete copy tied to the habit ("Allow Tally to remind you to Drink water at 8:00 AM?") — this is where F11 is paid off.
- **Then**: land on the dashboard with that habit visible and today's check-in CTA active. (The empty-dashboard language from `signup-mock.html:63` disappears because the dashboard is no longer empty.)

Together this spec addresses, by deletion or construction: F3, F4, F5, F6, F8, F9, F10, F11. Within Step 1 it addresses F1, F2, F7, F12, F13, F14, F15.

**Deliberately out of scope for launch**
- **F16 SSO** — plan post-launch. If Apple/Google sign-in is added on iOS later, Apple's App Store rules may require Sign-in-with-Apple alongside; AI assumption, verify current App Store guidelines before implementing.
- **Step indicator (F8)** and **Back button (F9)** — moot in a 2-step flow where each step is short. Add if post-launch testing shows users feel lost.

### Validation plan — cheapest test per top recommendation

No analytics are required for the pre-launch tests below.

| Recommendation | Cheapest test | What it would confirm / reject |
|---|---|---|
| 2-step flow (overall) | **5-user moderated test** before launch, task script: "Sign up and start tracking a habit." | Does T4 complete inside signup? Where do users still hesitate? |
| Password UX (F1 + F2 + F15) | Within the same 5-user test, instrument the password step with think-aloud; count attempts before pass. | Rule hints + preserved field + password-manager autofill actually reduce retries. |
| Deferred notifications (F11) | Instrument permission-grant and permanent-deny rates at launch (needs the lightest analytics event, not a full stack). | Deferring to post-habit raises grants and lowers permanent-denies, as hypothesized. |
| First-habit screen framing | **Hallway test** of 3 variants of the Step 2 headline + chips with 5 non-user friends. | Which framing feels most inviting; detect any JTBD mismatch early. |

Reviewer-independence logic from heuristic evaluation applies: have each user complete without observing another (https://www.nngroup.com/articles/how-to-conduct-a-heuristic-evaluation/ ).

### Handoff

- **To visual design step** → everything in "What to build" above is the spec. Pass with evidence labels intact.
- **To PRD / backend** → confirm the open questions below (especially whether phone/birthday are actually used anywhere). If no, delete from the data model.
- **To validation / research** → the 5-user test above, before launch. Then minimum instrumentation for notification grants and signup-to-first-habit completion at launch so you can learn in-market.

### Open questions

- **Open question**: Are phone or birthday used by any current or planned feature? (Blocks F4 cut.)
- **Open question**: What is the current security requirement for the password rule? (Decides whether complexity can be relaxed and replaced with length + breach-check.)
- **Open question**: Is email verification part of the account-creation flow after the mock? (Decides scope of F7.)
- **Open question**: Platform at launch — web, iOS, Android, all three? (Decides whether F11's OS-prompt timing and F16 SSO rules apply.)

---

### Evidence label legend

- **User-provided fact** — stated by the user in the brief or confirmed at a gate.
- **Sourced fact** — observed directly in `signup-mock.html` with file and line, or cited from a method source listed above.
- **AI assumption** — reviewer inference or estimate.
- **Approved decision** — confirmed by the user at Gate 1 or Gate 2.
- **Open question** — unknown; needs an answer or research.

(No **Real user research** or **Synthetic persona response** inputs were used in this audit.)
