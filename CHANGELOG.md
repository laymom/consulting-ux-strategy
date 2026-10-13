# Changelog

## 1.2.0 — 2026-10-13

### Added
- **Quick fix mode (new default).** Give it a screen or flow you built (screenshots preferred, code also works) and ask what to fix: you get the top 3 fixes on one screen, each with where it is, what goes wrong for the user, why (heuristic, severity 0–4 with a reason, source link, evidence label), and a fix instruction you can paste back into Claude Code, with a check for when it's done. Then one cheapest validation step and an offer to go deeper (Audit / Strategy)
- Quick fix looks first for problems a code review would not catch: next action not visible, unclear choices, information asked or given at the wrong time, missing feedback or progress, wording users won't understand, errors that punish the user
- Quick fix runs without gates because it only reports; anything assumed is labeled AI assumption, and it never invents what the user or their team said. With code only, it says so and makes no claims about how the screen looks
- Final check item 7 for Quick fix: at most 3 fixes; each instruction is concrete enough to paste (element, change, new wording, check)
- `examples/signup-step1.png`–`signup-step3.png`: rendered screenshots (390×844) of the mock signup flow
- `examples/signup-baseline-plain-chat.md`: the same screenshots and question in plain Claude Code without the skill, for comparison
- `examples/strategy-tally.md`: a real Strategy-mode run (fictional habit app "Tally", product brief only, no screens) with the conversation and the saved strategy document verbatim — framing, issue tree, JTBD, hypothetical first-week journey map, information architecture with a card-sort/tree-test plan, priorities, and next validation steps
- README: one line under the Quick fix example on how the 3 fixes are chosen (Nielsen heuristic evaluation, severity 0–4, the user's main task), and short real excerpts of Audit and Strategy output under "Want more depth?"

### Changed
- Positioning: from "UX audit and strategy reports" to "find what to fix first in a screen you built"; the skill description, intro, and README top were rewritten around that use. Audit, Strategy, and Full modes are unchanged and are now the deeper options
- The Final check now applies to every mode and asks to re-read the actual output, not check from memory
- `examples/signup-audit.md` is now a Quick fix run on the three screenshots; the previous Audit-mode run moved to `examples/signup-audit-full.md`
- The batch agent lists Quick fix among its modes
- README: use scene, Quick fix example, and install come first; methods and sources moved lower
- README title and first sentence name it as a UX audit (Nielsen heuristic evaluation, severity 0–4) so the UX method behind Quick fix is visible; Quick fix stays the default
- Final check: if a file was saved, it is re-opened with Read and fixed with Edit before the check is reported; superlatives about which screen or moment decides the outcome are removed even when labeled AI assumption; statements about what the target users did, felt, or said in the past need the user's own words or become hypotheses; hypothetical journey and empathy map thoughts are written as paraphrase, not quotes; participant counts and success thresholds in test plans need a source or are labeled AI assumption (found in a Strategy-mode run)

## 1.1.0 — 2026-10-12

### Added
- Mode selection (Audit / Strategy / Full) from the brief, with the reason stated. Only the methods that fit are applied, each with a one-line reason; a small screen check no longer forces a North Star metric or a full issue tree
- Shared evidence labels: User-provided fact, Sourced fact, Real user research, AI assumption, Synthetic persona response, Approved decision, Open question
- Per-issue fields: where observed, user task, problem, expected impact, related principle, severity with reason, fix, how to validate
- Observation-vs-inference rules: no drop-off causes or conversion lift claimed from a screen review; no visual-inspection claim when only code was read; accessibility comments state scope and tool and never imply WCAG certification; maps built from synthetic input are labeled "hypothetical (synthetic input)"
- A "next validation" step for top recommendations
- `CHANGELOG.md`
- `examples/signup-audit.md`: a real Claude Code run on a mock signup page (`examples/signup-mock.html`), with the conversation and report unedited and run notes on how it was made

### Changed
- `SKILL.md`, `agents/ux-strategist.md`, `references/handoff-map.md`, and `references/ux-methods.md` are now in English; reports follow the language of the user's input. No method, step, scale, or source link was removed
- The batch agent no longer hardcodes `~/.claude/skills/ux-strategy/`. It uses the reference path from its brief, then `./.claude/skills/ux-strategy/`, then `~/.claude/skills/ux-strategy/`
- Prioritization: with thin data, evidence-based qualitative priority is preferred over numeric ranking; RICE inputs without real Reach/Effort are marked as estimates; the "30+ issues → RICE" rule is softened to a rule of thumb
- README reordered: summary, example, install, input example, what happens, how it differs, limits, related tools, sources. The agent is documented as optional and Claude Code only, with separate install commands
- Wording: "consulting-grade" and "the methods consulting firms use" replaced with "consulting-style methods"; the claim that every method has a primary source was removed; author-specific tone instructions replaced with neutral wording

### Fixed (found in a real run)
- Final check now traces every user-attributed statement to the conversation and bans superlative cause rankings (the second real run attributed a statement to the team that they never made)
- Gates: an unanswered gate (non-interactive session, a question tool returning nothing) now stops with a statement of the approval needed; the skill no longer "proceeds with the default". A gate the brief already answers is stated as an assumption and still stops once, unless the user explicitly asked to run without stopping (then every unapproved choice is labeled AI assumption)
- User tasks the skill defines itself (e.g. T1–T4) are AI assumptions until approved at GATE 1
- Evidence labels: exactly one of the 7 names per claim (no combinations such as "AI assumption + JTBD inference"); direct observations of the user's provided files are Sourced fact, cited with file and line
- No URLs outside `references/ux-methods.md` or the user's own files; claims about industry norms, benchmarks, or "in <year>" need a source or become AI assumption
- No ranking of drop-off causes and no conversion claims ("killer", "likeliest", "will increase") from a screen review; such claims are written as hypotheses with a way to validate
- Method sources are cited where the method is applied (e.g. Pyramid Principle at the recommendation)
- New "Final check" section in `SKILL.md`, run on the finished report before sending; the batch agent runs it too
- `examples/signup-audit.md` re-run with this version

### Fixed (6b53ed4)
- Invocation is `/ux-strategy`, and install steps were corrected
- Sources added or corrected for Design Council (Double Diamond), Stanford d.school, Minto, HEART, and Strategyzer
- Secondary explainer links labeled as such
- Unverified numbers removed

## 1.0.0 — 2026-06-19

- Initial release: `ux-strategy` skill, `ux-strategist` batch agent, methods reference, and handoff map
