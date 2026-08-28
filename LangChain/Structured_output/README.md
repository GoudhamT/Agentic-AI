# Structured Output in LangChain

This document explains **Structured Output in LangChain**, with a focus on the two strategies used to obtain structured responses from models:

- **Provider Strategy**
- **Tool Strategy**

The notes use a cinema booking/cancellation example (`CineBot`) to explain when structured output is useful and how the two strategies differ.

## What is Structured Output?

Structured output is used when you want the model to return its response in a **specific format** instead of unrestricted natural language.

For example, in a cinema booking or cancellation application, you may want every customer request to be converted into a predictable structure that your application can process.

> Structured output helps make model responses predictable and easier for an application to consume.

However, simply asking a model to return a particular format does not guarantee that the response will always follow that format. The strategy used by LangChain depends on model capabilities and how structured output is configured.

## Two Structured Output Strategies

LangChain uses two main approaches:

### 1. Tool Strategy

With the **Tool Strategy**, structured output is achieved through a synthetic/fake tool call.

According to the notes:

- It works even when the model does not natively support structured output.
- It is generally **slower**.
- It is configured at the **agent level** when the model does not support structured output.
- Despite the name, it is **not intended to call an application tool**. It is used to obtain structured output when native structured output is unavailable.

Conceptually:

```text
Customer Request
       |
       v
     Agent
       |
       v
 Synthetic Tool Call
       |
       v
Structured Response
```

### 2. Provider Strategy

The **Provider Strategy** uses the model/provider's native structured-output capability.

According to the notes:

- It is automatically used by LangChain when a schema is passed directly to `create_agent` through `response_format`.
- The model must support native structured output.
- It is generally **faster** than the Tool Strategy.
- It only works when the selected model/provider supports the required structured-output capability.

Conceptually:

```text
Customer Request
       |
       v
     Agent
       |
       v
Native Provider Structured Output
       |
       v
Structured Response
```

## Provider Strategy vs Tool Strategy

| Feature | Provider Strategy | Tool Strategy |
|---|---|---|
| How it works | Uses native provider structured output | Uses a synthetic tool call |
| Speed | Faster | Slower |
| Model requirement | Model must support structured output | Used when model does not support structured output |
| LangChain usage | `response_format` at agent level or `.with_structured_output()` on a supported model | `response_format` at agent level |
| Actual application tool call | No | No |
| Main purpose | Native structured responses | Structured responses when native support is unavailable |

## `.with_structured_output()` vs `response_format`

The notes highlight an important distinction in how structured output can be configured.

### `.with_structured_output()`

When you use:

```python
model.with_structured_output(...)
```

on a model that supports structured output, this represents the **Provider Strategy**.

The structured-output configuration is applied directly to the model.

### `response_format` in an Agent

When you use:

```python
create_agent(
    model=model,
    response_format=...
)
```

LangChain can select the appropriate strategy based on model support.

If the model supports native structured output, LangChain uses the **Provider Strategy**.

If the model does not support native structured output, the **Tool Strategy** can be used.

## Key Point About Tool Strategy

One of the most important points in the notes is that **Tool Strategy does not mean you are asking the model to execute one of your normal tools**.

Instead, LangChain uses a synthetic tool-call mechanism to represent the structured response.

So:

```text
Tool Strategy
     |
     +--> Structured output mechanism
     |
     +--> NOT necessarily an application/business tool
```

This is especially useful when the model cannot provide structured output natively.

## CineBot Example

The source material uses a cinema booking/cancellation scenario called **CineBot**.

A customer may send a request such as:

```text
Book movie tickets for me.
```

Instead of relying on an unrestricted text response, the application can define a schema for the expected result.

The goal is to transform the customer's natural-language request into a predictable structured response that the application can work with.

For example, the application could conceptually expect information such as:

```text
Customer
Movie
Number of tickets
Action
```

The exact schema should be defined by the application.

## Decision Flow

A simple way to remember the strategy selection is:

```text
                  Structured Output Needed
                           |
                           v
                 Does the model support
                 native structured output?
                     /                                Yes              No
                    |                |
                    v                v
           Provider Strategy    Tool Strategy
                    |                |
                    v                v
             Native output    Synthetic tool call
```

## Quick Rules

### Use Provider Strategy when:

- The model supports native structured output.
- You want the provider to enforce/produce the structured response.
- You want the faster native approach.

### Use Tool Strategy when:

- The model does not support native structured output.
- You still need a structured response from the agent.
- You are configuring structured output at the agent level.

## Important Takeaways

1. **Structured output is about controlling the format of the model's response.**
2. LangChain provides **Provider Strategy** and **Tool Strategy**.
3. **Provider Strategy** uses native structured-output support from the model/provider.
4. **Tool Strategy** uses a synthetic tool call to obtain structured output.
5. Provider Strategy is generally **faster**, but requires model support.
6. Tool Strategy works when native structured output is not supported.
7. `.with_structured_output()` on a supported model represents the **Provider Strategy**.
8. `response_format` in an agent allows LangChain to select the appropriate strategy based on model capabilities.
9. **Tool Strategy is not the same thing as calling an application tool**; its purpose here is structured output.

## Reference

These notes are based on the accompanying document covering Structured Output, Provider Strategy, Tool Strategy, and the CineBot example.
