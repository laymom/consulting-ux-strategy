# Example run: "What should I fix first?" (Quick fix mode)

A real run of `/ux-strategy` in Claude Code on three screenshots of a mock signup flow. For the full review with gates (Audit mode), see [signup-audit-full.md](signup-audit-full.md).

| | |
|---|---|
| Input | `/ux-strategy I built this signup flow for a habit-tracking app (screenshots: signup-step1.png, signup-step2.png, signup-step3.png). What should I fix first?` |
| Screenshots | [signup-step1.png](signup-step1.png), [signup-step2.png](signup-step2.png), [signup-step3.png](signup-step3.png) — rendered from [signup-mock.html](signup-mock.html) |
| Mode chosen by the skill | Quick fix (the default when a screen is given and no depth is asked for) |
| Model (from CLI JSON) | `claude-opus-4-7[1m]` (plus a small `claude-haiku-4-5` side call) |
| Skill version | 1.2.0 — this commit's `SKILL.md` |
| Claude Code version | 2.1.142 |
| Run date | 2026-10-13 |
| Environment | Fresh project-level install from a local clone in an empty temp folder; personal CLAUDE.md, user-level skills and settings, and MCP servers disabled |
| Turns | 1 user turn, no follow-up |
| Wall time (from CLI JSON) | 82.5 s |

| Step 1 | Step 2 | Step 3 |
|---|---|---|
| [<img src="signup-step1.png" width="160" alt="Step 1: Create your account — email and password">](signup-step1.png) | [<img src="signup-step2.png" width="160" alt="Step 2: Tell us about you — phone number and birthday">](signup-step2.png) | [<img src="signup-step3.png" width="160" alt="Step 3: Turn on notifications — Allow button and a small Skip link">](signup-step3.png) |

## Run notes

- **"Tally", the app, and the signup page are fictional mocks** made for this example.
- **Screenshots:** each step of `signup-mock.html` rendered with headless Chrome at a 390 × 844 mobile viewport (a temporary copy of the page showed the step and fixed the layout width at 390 px; the mock itself is unchanged). The model saw only the three PNG files — not the HTML, so it could not see validation rules, error messages, or the final screen.
- **How it was run:** `claude -p` from an empty temp folder after `npx -y skills add <local clone> --skill ux-strategy -a claude-code -y`, with `CLAUDE_CODE_DISABLE_CLAUDE_MDS=1`, `--setting-sources project --strict-mcp-config --permission-mode acceptEdits --allowedTools "Agent,Task,Read,Write,Edit,Glob,Grep"`; Bash, web search/fetch, and `AskUserQuestion` disabled; reading the author's user-level `~/.claude` folder and the repo clone denied. A probe call first confirmed that the only `ux-strategy` visible was the freshly installed one and that no CLAUDE.md was loaded. The installer also copies this repo's `README.md`, `CHANGELOG.md`, and `examples/` into the install; they were deleted from the install before the run so the model could not read earlier examples.
- **An earlier run of this input with a draft of 1.2.0** wrote abandonment and opt-in claims, invented a product fact ("a free habit app"), and reworded the notification screen instead of moving the permission ask to a later moment (the plain-chat answer did move it). Those rules were tightened in `SKILL.md` before this run (see [CHANGELOG 1.2.0](../CHANGELOG.md)).
- **Output is not edited.** The answer below is verbatim. Tool calls (reading `references/ux-methods.md`, finding and reading the three PNGs) are omitted.
- **Read critically:** fix 1's severity reason still says the field is "likely to make users distrust or abandon", which the skill's rules ask to state as a hypothesis; fix 3's "users agree to invasive fields partly because…" is an unlabeled inference; and fix 3 assumes a "Set up your first habit" step that fix 2 implies but no screenshot shows.

## Answer (verbatim)

> Quick fix — I looked at the three screenshots (signup-step1.png, signup-step2.png, signup-step3.png) for the task "sign up and start tracking a habit". No PRD was provided, so the user task is an AI assumption.
>
> 1. **Step 2 (Tell us about you) — Phone number asked before the user has seen any product value, with no reason given.** The user came to track habits and is being asked for a sensitive personal field before using the app once; phone number is unexpected for a habit tracker and lowers trust.
>    Why: Nielsen H2 Match system to the real world + H10 Help and documentation (https://www.nngroup.com/articles/ten-usability-heuristics/) · Severity 4: likely to make users distrust or abandon before entering the product · Sourced fact (signup-step2.png)
>    Fix instruction for Claude Code:
>    > Remove the Phone number field from the signup flow and move it to Profile Settings as optional. If it must stay in signup, mark it `Phone number (optional)` and add helper text `<one-line reason the user needs to give a phone number — fill in or delete if there is no clear reason>`. Do the same check for Birthday: keep it only if there is a product reason (e.g., age-based defaults); otherwise remove or mark optional with helper text.
>    > Check: On step 2 after this change, no field is required unless it has a visible one-line reason, and the user can submit the step without entering a phone number.
>
> 2. **Step 3 (Turn on notifications) — Permission asked before any habit or reminder exists.** The user has not set up a habit yet, so there is nothing to be notified about; the screen gives no reason, and once the OS prompt is denied, re-asking is blocked on most platforms (AI assumption — platform behavior varies).
>    Why: Nielsen H1 Visibility of system status + H10 Help and documentation (https://www.nngroup.com/articles/ten-usability-heuristics/) · Severity 4: a permission denied here is hard to recover and breaks the core reminder feature · Sourced fact (signup-step3.png)
>    Fix instruction for Claude Code:
>    > Delete the standalone "Turn on notifications" step from signup. After the user creates their first habit and picks a reminder time, show the native OS permission prompt from that screen, preceded by one line: `Allow notifications so Tally can remind you at <reminder time> to <habit name>.` Keep a `Not now` option that leaves the habit saved without reminders.
>    > Check: A new user can finish signup without seeing a notification prompt; the prompt only appears after they save a habit with a reminder, and the message names that habit and time.
>
> 3. **All 3 screens — No progress indicator, so the user doesn't know how many steps are left or what will be asked next.** This also amplifies fix 1: users agree to invasive fields partly because they don't know if "just one more step" is coming.
>    Why: Nielsen H1 Visibility of system status (https://www.nngroup.com/articles/ten-usability-heuristics/) · Severity 3: does not block the task but adds anxiety across every step · Sourced fact (signup-step1.png, signup-step2.png, signup-step3.png)
>    Fix instruction for Claude Code:
>    > After applying fixes 1 and 2, signup is 2 steps: (1) Create account, (2) Set up your first habit. Add a progress indicator directly under the Tally logo on each step reading `Step 1 of 2` and `Step 2 of 2` (left-aligned, same style as the field labels). Also add a `Back` text link in the top-left on step 2 that returns to step 1 without clearing entered values.
>    > Check: On each signup screen the user can see which step they are on and the total, and on step 2 tapping Back returns to step 1 with email and password still filled in.
>
> Next: validate with 5 people who have never used Tally — ask them to think aloud while they "sign up and set your first habit", and note where they hesitate or ask "why do you need this?".
> Want the full audit (every issue, cognitive walkthrough, priorities) or a strategy (structure, journeys)? Say "Audit" or "Strategy".

## Compared with plain chat (same screenshots, no skill)

Same three screenshots and the same question, in a separate empty folder with the same isolation settings and no skill installed: [signup-baseline-plain-chat.md](signup-baseline-plain-chat.md).

**Only the skill run produced:**
- A fix instruction per problem that can be pasted into Claude Code as is, with exact new wording (`Phone number (optional)`, `Step 1 of 2`, the reminder sentence) and a "Check" line saying how to tell the change is done.
- Fixes written as one set: fix 3 uses the 2-step flow that fixes 1 and 2 create, and adds a Back link that keeps entered values.
- A third problem the plain answer did not raise: no progress indicator on any screen.
- For each problem: the heuristic it breaks, a severity 0–4 with a reason, a source link, and an evidence label; the assumed user task is stated and labeled.
- A placeholder (`<one-line reason the user needs to give a phone number…>`) instead of a made-up reason.
- One cheapest validation step (5 first-time users thinking aloud).

**Plain chat did as well or better:**
- It found the same top two problems (phone and birthday asked too early; notification permission asked before any habit exists) and gave the same core fix for the second (ask after the first habit or reminder is set).
- It was shorter (two paragraphs) and faster (about 38 s vs. 82 s).
- It named a trade-off the skill did not: dropping the profile fields loses some early marketing or analytics data.

**Where plain chat was weaker:** it stated outcomes as facts without a source ("the biggest conversion killer", "materially more completed signups", denial "is permanent" on iOS/web), and gave no wording, no acceptance check, and no way to validate.
