# Baseline: the same question in plain Claude Code (no skill)

Comparison run for [signup-audit.md](signup-audit.md). Same three screenshots, same question without `/ux-strategy`.

| | |
|---|---|
| Input | `I built this signup flow for a habit-tracking app (screenshots: signup-step1.png, signup-step2.png, signup-step3.png). What should I fix first?` |
| Screenshots | [signup-step1.png](signup-step1.png), [signup-step2.png](signup-step2.png), [signup-step3.png](signup-step3.png) |
| Skill | None installed |
| Model (from CLI JSON) | `claude-opus-4-7[1m]` (plus a small `claude-haiku-4-5` side call) |
| Claude Code version | 2.1.142 |
| Run date | 2026-10-13 |
| Environment | Empty temp folder with no project skills; personal CLAUDE.md, user-level skills and settings, and MCP servers disabled; same flags as the skill run |
| Turns | 1 user turn |
| Wall time (from CLI JSON) | 37.9 s |

The app and page are fictional mocks. The answer is verbatim; tool calls (finding and reading the three PNGs) are omitted.

## Answer (verbatim)

> Fix **step 2** first — asking a brand-new user for phone number and birthday before they've seen any value is the biggest conversion killer here, and a habit tracker has no obvious reason to need either upfront. Either drop that step entirely or defer those fields to a profile screen after the user has created their first habit; the tradeoff is losing some early marketing/analytics data in exchange for materially more completed signups.
>
> Close second: step 3's notification prompt fires before the user has done anything, so most people will deny it — and on iOS/web that denial is permanent. Trigger the OS permission request later, right after they create a habit or set their first reminder time.
