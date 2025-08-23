# GPT-5 prompting guide

## Agentic workflow predictability

We trained GPT-5 with developers in mind: we’ve focused on improving tool calling, instruction following, and long-context understanding to serve as the best foundation model for agentic applications.

_If adopting GPT-5 for agentic and tool calling flows, we recommend upgrading to the Responses API, where reasoning is persisted between tool calls, leading to more efficient and intelligent outputs._

### Prompting for less eagerness

- Switch to a lower reasoning_effort. This reduces exploration depth but improves efficiency and latency. Many workflows can be accomplished with consistent results at medium or even low reasoning_effort.
- Define clear criteria in your prompt for how you want the model to explore the problem space. This reduces the model’s need to explore and reason about too many ideas:

```
<context_gathering>
Goal: Get enough context fast. Parallelize discovery and stop as soon as you can act.

Method:

- Start broad, then fan out to focused subqueries.
- In parallel, launch varied queries; read top hits per query. Deduplicate paths and cache, and don’t repeat queries.
- Avoid over searching for context. If needed, run targeted searches in one parallel batch.

Early stop criteria:

- You can name exact content to change.
- Top hits converge (~70%) on one area/path.

Escalate once:

- If signals conflict or scope is fuzzy, run one refined parallel batch, then proceed.

Depth:

- Trace only symbols you’ll modify or whose contracts you rely on; avoid transitive expansion unless necessary.

Loop:

- Batch search → minimal plan → complete task.
- Search again only if validation fails or new unknowns appear. Prefer acting over more searching.
  </context_gathering>

```

If you’re willing to be maximally prescriptive, you can even set fixed tool call budgets, like the one below. The budget can naturally vary based on your desired search depth.

```

<context_gathering>

- Search depth: very low
- Bias strongly towards providing a correct answer as quickly as possible, even if it might not be fully correct.
- Usually, this means an absolute maximum of 2 tool calls.
- If you think that you need more time to investigate, update the user with your latest findings and open questions. You can proceed if the user confirms.
  </context_gathering>

```

we recommend increasing reasoning_effort, and using a prompt like the following to encourage persistence and thorough task completion:

```

<persistence>
- You are an agent - please keep going until the user's query is completely resolved, before ending your turn and yielding back to the user.
- Only terminate your turn when you are sure that the problem is solved.
- Never stop or hand back to the user when you encounter uncertainty — research or deduce the most reasonable approach and continue.
- Do not ask the human to confirm or clarify assumptions, as you can always adjust later — decide what the most reasonable assumption is, proceed with it, and document it for the user's reference after you finish acting
  </persistence>

```

_It can be helpful to clearly state the stop conditions of the agentic tasks, outline safe versus unsafe actions, and define when, if ever, it’s acceptable for the model to hand back to the user._

### Tool preambles

You can steer the frequency, style, and content of tool preambles in your prompt—from detailed explanations of every single tool call to a brief upfront plan and everything in between.

This is an example of a high-quality preamble prompt:

```

<tool_preambles>

- Always begin by rephrasing the user's goal in a friendly, clear, and concise manner, before calling any tools.
- Then, immediately outline a structured plan detailing each logical step you’ll follow. - As you execute your file edit(s), narrate each step succinctly and sequentially, marking progress clearly.
- Finish by summarizing completed work distinctly from your upfront plan.
  </tool_preambles>

```
