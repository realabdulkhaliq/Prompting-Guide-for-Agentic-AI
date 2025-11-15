# Prompt and Context Engineering

### How LLM Work

When an LLM answers a question, it produces text one small piece at a time. The technical name for a piece is token.1 Tokens can be words or parts of words. At each step, the LLM predicts which token should come next based on the prompt and what it has already written so far.2

An external system runs the LLM in a “generate the next token; append it to the input; generate the next token” loop until a stopping condition is triggered. When this happens, the system stops asking the LLM for more tokens and shows the result to the user.

Many stopping conditions are used in practice. An important one involves a special “**end of sequence**” token that (informally) means “end of answer.” This token is used in the training process to denote the end of individual training examples and so, during training, the LLM learns to predict this special token at the point where its answer is complete. Other stopping conditions include (but are not limited to) a limit on the maximum number of tokens that have been generated so far, or the generation of a user-defined pattern called a stop sequence.

When your organization starts building its own LLM apps, developers can adjust these stopping rules and other parameters themselves, and these choices can affect answer completeness, cost, and formatting.

Without access to live data, a model might still generate an answer based on its training data that doesn’t reflect real-world updates.
