# Prompt and Context Engineering

### How LLM Work

When an LLM answers a question, it produces text one small piece at a time. The technical name for a piece is token.1 Tokens can be words or parts of words. At each step, the LLM predicts which token should come next based on the prompt and what it has already written so far.2

An external system runs the LLM in a “generate the next token; append it to the input; generate the next token” loop until a stopping condition is triggered. When this happens, the system stops asking the LLM for more tokens and shows the result to the user.
