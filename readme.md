## Structured Output in LangChain

This section explores how LangChain handles **structured output** when an application needs model responses in a predefined format.

It covers two main strategies:

- **Provider Strategy** – uses the model/provider's native structured-output capability when supported.
- **Tool Strategy** – uses a synthetic tool-call approach when native structured output is not supported.

The examples use a **cinema booking/cancellation use case (CineBot)** to demonstrate how structured responses can be used in an agent.

### Topics Covered

- Structured Output
- Provider Strategy
- Tool Strategy
- `.with_structured_output()`
- `response_format`
- Provider vs Tool Strategy
- Structured responses in agents

📂 [View Structured Output](./LangChain/Structured_output)