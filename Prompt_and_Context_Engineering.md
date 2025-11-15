# Prompt and Context Engineering

### How LLM Work

When an LLM answers a question, it produces text one small piece at a time. The technical name for a piece is token.1 Tokens can be words or parts of words. At each step, the LLM predicts which token should come next based on the prompt and what it has already written so far.2

An external system runs the LLM in a “generate the next token; append it to the input; generate the next token” loop until a stopping condition is triggered. When this happens, the system stops asking the LLM for more tokens and shows the result to the user.

Many stopping conditions are used in practice. An important one involves a special “**end of sequence**” token that (informally) means “end of answer.” This token is used in the training process to denote the end of individual training examples and so, during training, the LLM learns to predict this special token at the point where its answer is complete. Other stopping conditions include (but are not limited to) a limit on the maximum number of tokens that have been generated so far, or the generation of a user-defined pattern called a stop sequence.

When your organization starts building its own LLM apps, developers can adjust these stopping rules and other parameters themselves, and these choices can affect answer completeness, cost, and formatting.

Without access to live data, a model might still generate an answer based on its training data that doesn’t reflect real-world updates.

**When we have many documents, we use RAG, where we first gather relevant information from documents and include only those in the prompt. But modern LLMs have long context windows, and we can easily include all the documents. Is RAG even necessary?**

Modern LLMs like GPT-4.1 and Gemini 2.5 offer million-token context windows — enough to hold entire books. This naturally raises the question: If we can fit everything in, why bother using a subset?

While these extended context windows are powerful, including all documents in the prompt isn’t always a good idea. There are several reasons why RAG still matters.

First, RAG isn’t just about keeping the prompt short. It’s about selecting the most relevant parts of the documents. Overloading the context with too much or irrelevant information can hurt performance, and keeping the context and prompt relevant, concise, and accurate often leads to better answers.

Second, even though LLMs can accept long contexts, they don’t process all parts equally well. Research has shown that AI models tend to focus more on the beginning and end of a prompt and may miss important information in the middle.

Finally, longer prompts mean more tokens, which increases API costs and slows down responses. This matters in real-world applications where cost and speed are important.

Hallucinations cannot be fully eliminated with current LLM technology. They arise from the probabilistic nature of language models, which generate text by predicting likely token sequences based on training data. But careful prompt engineering and strategies such as RAG, fine-tuning on domain-specific data, and post-processing with rule-based checks or external validation can reduce hallucinations in specific use cases.

An increasingly popular alternative is to use an “AI judge,” which is typically another LLM that can evaluate or verify the outputs of the first tool. This approach allows for scalable and fast accuracy-checking, but it comes with limitations: The judge itself may hallucinate or fail to match human judgment, particularly in complex cases. Some improvements include using multiple judges for comparison, combining judge feedback with retrieval-based fact-checking, or designing workflows where low-confidence outputs are escalated to humans.
