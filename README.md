# Prompting-Guide-for-Agentic-AI

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

### 🛠️ Tool Usage Best Practices

- Use the tools field in API requests rather than injecting tool descriptions manually.

- Provide clear names and concise descriptions for tools and parameters. (Developers should name tools clearly to indicate their purpose and add a clear, detailed description in the "description" field of the tool.)

- Include examples in a separate section to guide tool usage.
  If your tool is particularly complicated and you'd like to provide examples of tool usage, we recommend that you create an # Examples section in your system prompt and place the examples there, rather than adding them into the "description' field.
  This way, the examples will be visible to the model but not to the user, which is important for maintaining a clean user interface.
