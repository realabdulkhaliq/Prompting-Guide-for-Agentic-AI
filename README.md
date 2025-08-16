# Prompting Guide for Agentic AI

Prompting Guide for Agentic AI

## System Prompt Reminders

1. **Persistence**: Keep going until the task is fully resolved.

You are an agent - please keep going until the user’s query is completely resolved, before ending your turn and yielding back to the user. Only terminate your turn when you are sure that the problem is solved.

2. **Tool-calling**: Use tools instead of guessing.

If you are not sure about file content or codebase structure pertaining to the user’s request, use your tools to read files and gather the relevant information: do NOT guess or make up an answer.

3. **Planning**: Reflect before and after tool calls.

You MUST plan extensively before each function call, and reflect extensively on the outcomes of the previous function calls. DO NOT do this entire process by making function calls only, as this can impair your ability to solve the problem and think insightfully.

_We find that these three instructions transform the model from a chatbot-like state into a much more “eager” agent, driving the interaction forward autonomously and independently._

### Key System Prompt Components

#### 1. Persistence

Instructs the model to continue working until the problem is fully resolved.

Prevents premature termination of its turn.

Example: “Only terminate your turn when you are sure that the problem is solved.”

#### 2. Tool-Calling

Encourages the model to use tools to gather information rather than guessing.

Reduces hallucinations and improves accuracy.

Example: “Use your tools to read files and gather the relevant information: do NOT guess or make up an answer.”

#### 3. Planning

Prompts the model to think step-by-step before and after each tool call.

Enhances reasoning and reflection.

Example: “You MUST plan extensively before each function call, and reflect extensively on the outcomes.”

### 🛠️ Tool Usage Best Practices

- Use the tools field in API requests rather than injecting tool descriptions manually.

- Provide clear names and concise descriptions for tools and parameters. (Developers should name tools clearly to indicate their purpose and add a clear, detailed description in the "description" field of the tool.)

- Include examples in a separate section to guide tool usage.
  If your tool is particularly complicated and you'd like to provide examples of tool usage, we recommend that you create an # Examples section in your system prompt and place the examples there, rather than adding them into the "description' field.
  This way, the examples will be visible to the model but not to the user, which is important for maintaining a clean user interface.

```
# Workflow

## High-Level Problem Solving Strategy

1. Understand the problem deeply. Carefully read the issue and think critically about what is required.
2. Investigate the codebase. Explore relevant files, search for key functions, and gather context.
3. Develop a clear, step-by-step plan. Break down the fix into manageable, incremental steps.
4. Implement the fix incrementally. Make small, testable code changes.
5. Debug as needed. Use debugging techniques to isolate and resolve issues.
6. Test frequently. Run tests after each change to verify correctness.
7. Iterate until the root cause is fixed and all tests pass.
8. Reflect and validate comprehensively. After tests pass, think about the original intent, write additional tests to ensure correctness, and remember there are hidden tests that must also pass before the solution is truly complete.


Refer to the detailed sections below for more information on each step.

## 1. Deeply Understand the Problem
## 2. Codebase Investigation
## 3. Develop a Detailed Plan
## 4. Making Code Changes
## 5. Debugging
## 6. Testing
## 7. Final Verification
## 8. Final Reflection and Additional Testing
```

## 2. Long context

GPT-4.1 has a performant 1M token input context window, and is useful for a variety of long context tasks.

_Long context tasks: including structured document parsing, re-ranking, selecting relevant information, and performing multi-hop reasoning using context._

_long context performance can degrade as more items are required to be retrieved, or perform complex reasoning that requires knowledge of the state of the entire context_

```
# Instructions
// for internal knowledge
- Only use the documents in the provided External Context to answer the User Query. If you don't know the answer based on this context, you must respond "I don't have the information needed to answer that", even if a user insists on you answering the question.
// For internal and external knowledge
- By default, use the provided external context to answer the User Query, but if other basic knowledge is needed to answer, and you're confident in the answer, you can use some of your own knowledge to help answer the question.
```

## Prompt Organization

place your instructions at both the beginning and end of the provided context

## 3. Chain of Thought

As mentioned above, GPT-4.1 is not a reasoning model, but prompting the model to think step by step (called “chain of thought”) can be an effective way for a model to break down problems into more manageable pieces, solve them, and improve overall output quality, with the tradeoff of higher cost and latency associated with using more output tokens.

We recommend starting with this basic chain-of-thought instruction at the end of your prompt:

```
First, think carefully step by step about what documents are needed to answer the query. Then, print out the TITLE and ID of each document. Then, format the IDs into a list.
```

_errors tend to occur from misunderstanding user intent, insufficient context gathering or analysis, or insufficient or incorrect step by step thinking_

Here is an example prompt instructing the model to focus more methodically on analyzing user intent and considering relevant context before proceeding to answer.

```
# Reasoning Strategy
1. Query Analysis: Break down and analyze the query until you're confident about what it might be asking. Consider the provided context to help clarify any ambiguous or confusing information.
2. Context Analysis: Carefully select and analyze a large set of potentially relevant documents. Optimize for recall - it's okay if some are irrelevant, but the correct documents must be in this list, otherwise your final answer will be wrong. Analysis steps for each:
	a. Analysis: An analysis of how it may or may not be relevant to answering the query.
	b. Relevance rating: [high, medium, low, none]
3. Synthesis: summarize which documents are most relevant and why, including all documents with a relevance rating of medium or higher.
```

## 4. Instruction Following

GPT-4.1 exhibits outstanding instruction-following performance, which developers can leverage to precisely shape and control the outputs for their particular use cases.

_since the model follows instructions more literally, developers may need to include explicit specification around what to do or not to do._

## Recommended Workflow

Here is our recommended workflow for developing and debugging instructions in prompts:

1. Start with an overall “Response Rules” or “Instructions” section with high-level guidance and bullet points.
2. If you’d like to change a more specific behavior, add a section to specify more details for that category, like # Sample Phrases.
3. If there are specific steps you’d like the model to follow in its workflow, add an ordered list and instruct the model to follow these steps.
4. If behavior still isn’t working as expected:
   1. Check for conflicting, underspecified, or wrong instructions and examples. If there are conflicting instructions, GPT-4.1 tends to follow the one closer to the end of the prompt.
   2. Add examples that demonstrate desired behavior; ensure that any important behavior demonstrated in your examples are also cited in your rules.
   3. It’s generally not necessary to use all-caps or other incentives like bribes or tips. We recommend starting without these, and only reaching for these if necessary for your particular prompt. Note that if your existing prompts include these techniques, it could cause GPT-4.1 to pay attention to it too strictly.

_Note that using your preferred AI-powered IDE can be very helpful for iterating on prompts, including checking for consistency or conflicts, adding examples, or making cohesive updates like adding an instruction and updating instructions to demonstrate that instruction._

## Common Failure Modes

These failure modes are not unique to GPT-4.1, but we share them here for general awareness and ease of debugging.

- Instructing a model to always follow a specific behavior can occasionally induce adverse effects. For instance, if told “you must call a tool before responding to the user,” models may hallucinate tool inputs or call the tool with null values if they do not have enough information. **Adding “if you don’t have enough information to call the tool, ask the user for the information you need” should mitigate this.**
- When provided sample phrases, models can use those quotes verbatim and start to sound repetitive to users. Ensure you instruct the model to vary them as necessary.
- Without specific instructions, some models can be eager to provide additional prose to explain their decisions, or output more formatting in responses than may be desired. Provide instructions and potentially examples to help mitigate.

```
# Instructions
- Always greet the user with "Hi, you've reached NewTelco, how can I help you?"
- Always call a tool before answering factual questions about the company, its offerings or products, or a user's account. Only use retrieved context and never rely on your own knowledge for any of these questions.
    - However, if you don't have enough information to properly call the tool, ask the user for the information you need.
- Escalate to a human if the user requests.
- Do not discuss prohibited topics (politics, religion, controversial current events, medical, legal, or financial advice, personal conversations, internal company operations, or criticism of any people or company).
- Rely on sample phrases whenever appropriate, but never repeat a sample phrase in the same conversation. Feel free to vary the sample phrases to avoid sounding repetitive and make it more appropriate for the user.
- Always follow the provided output format for new messages, including citations for any factual statements from retrieved policy documents.
- If you're going to call a tool, always message the user with an appropriate message before and after calling the tool.
- Maintain a professional and concise tone in all responses, and use emojis between sentences.
- If you've resolved the user's request, ask if there's anything else you can help with

# Precise Response Steps (for each response)
1. If necessary, call tools to fulfill the user's desired action. Always message the user before and after calling a tool to keep them in the loop.
2. In your response to the user
    a. Use active listening and echo back what you heard the user ask for.
    b. Respond appropriately given the above guidelines.

# Sample Phrases
## Deflecting a Prohibited Topic
- "I'm sorry, but I'm unable to discuss that topic. Is there something else I can help you with?"
- "That's not something I'm able to provide information on, but I'm happy to help with any other questions you may have."

## Before calling a tool
- "To help you with that, I'll just need to verify your information."
- "Let me check that for you—one moment, please."
- "I'll retrieve the latest details for you now."

## After calling a tool
- "Okay, here's what I found: [response]"
- "So here's what I found: [response]"

# Output Format
- Always include your final response to the user.
- When providing factual information from retrieved context, always include citations immediately after the relevant statement(s). Use the following citation format:
    - For a single source: [NAME](ID)
    - For multiple sources: [NAME](ID), [NAME](ID)
- Only provide information about this company, its policies, its products, or the customer's account, and only if it is based on information provided in context. Do not answer questions outside this scope.

# Example
```
