# UX Strategy Handoff Map

This skill covers "what, and how it flows". The next steps go elsewhere. Before handing off, confirm with the user in one line: "Should this go to ___ next?"

## Position in the value chain

```text
[Business review]    market entry decision (go / no-go)
     │ go
     ▼
[PRD]                requirements (what / why, user stories, acceptance criteria)
     │
     ▼
[ux-strategy] ←───── here. IA, journeys, diagnosis, strategy (how it flows)
     │
     ▼
[Visual design]      visual mockups (how it looks)
     │
     ▼
[Visual review]      polish and aesthetics
```

Each of these steps can be its own workflow (a slash command, an agent, a design tool — whatever fits your setup). This skill fills the **flow design** slot in that chain.

## Inputs (where work comes from)

- **From a PRD**: when requirements are done and UX strategy or IA is needed. Use the PRD as input.
- **After a go decision**: to set the product's UX direction.
- **Direct invocation**: `/ux-strategy` or an automatic trigger, including audits of existing screens.
- **Qualitative feedback**: persona feedback as input to empathy and journey maps. Keep its evidence label: synthetic persona output (e.g. from https://github.com/laymom/synthetic-focus-group ) stays labeled **Synthetic persona response**, and maps built from it are titled "hypothetical (synthetic input)". Only research with real users counts as **Real user research**.

## Outputs (where work goes)

| Next need | Hand off to | What to pass |
|---|---|---|
| Screen visuals, HTML mockups | Visual design step | IA structure, key flows, UX requirements per screen |
| Formal requirements | PRD step | Approved UX strategy and priorities |
| Business or market re-check | Business review step | Structural problems found in the UX diagnosis |
| Qualitative feedback | Focus group or user interviews | The IA and flows to be tested |
| External facts, benchmarks | Research step | Competitor UX patterns, statistics |

Keep the evidence labels (User-provided fact, Sourced fact, Real user research, AI assumption, Synthetic persona response, Approved decision, Open question) on everything you pass along.

## Relation to the batch agent (ux-strategist)

- Interactive, with gates → **this skill**
- Another workflow wants a one-shot diagnosis in an isolated context, or several lenses in parallel → **ux-strategist agent** (same `ux-methods.md`, no gates)

## Avoiding overlap

- "Make it pretty", colors, layout, components → not this skill. Point to the visual design step.
- "Evaluate it aesthetically" → visual review step.
- This skill is for **usability, structure, flow, strategy, and metrics**.
