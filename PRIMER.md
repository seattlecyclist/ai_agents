# AI Agents: A Primer

A fast on-ramp for the ideas you need before building the 30 agents in this course. Read it once now, then come back to it when a lesson leans on a concept.

---

## 1. What is an "agent," really?

A plain LLM call is a function: text in, text out. It cannot look anything up, cannot take an action, and forgets everything when the call ends.

An **agent** is an LLM wrapped in a loop that lets it act on the world and see what happened:

```
            ┌──────────────────────────────────────────┐
            │                                          │
  goal ──▶ THINK ──▶ ACT (call a tool) ──▶ OBSERVE ────┘
             │
             └──▶ done? ──▶ answer
```

That loop is the whole trick. Everything else in this course (memory, planning, multi-agent teams, guardrails) is a way of making that loop smarter, safer, or more reliable.

A useful working definition:

> **An agent is a system where the model decides what to do next, rather than the programmer.**

If you write `step1(); step2(); step3();` and call the model in each step, that is a **workflow**. If the model chooses which step to take, and when to stop, that is an **agent**. Both are useful. Good engineers reach for the simplest one that works, and the course teaches both.

## 2. The six building blocks

Almost every agent is assembled from the same parts:

| Block | What it does | Everyday analogy |
|---|---|---|
| **Model** | Reasons, writes, decides | The brain |
| **Instructions** (system prompt) | Role, rules, tone, goals | The job description |
| **Tools** | Functions the model can call: search, run code, send email | Hands |
| **Memory** | What the agent remembers within a task and across tasks | A notebook |
| **Planning** | Breaking a big goal into steps, revising as it goes | A to-do list |
| **Guardrails** | Checks on inputs, outputs, and actions | A supervisor |

## 3. Tool use (function calling), the core mechanic

Modern model APIs let you describe tools as JSON schemas. The model does not run the tool itself. It replies with a structured request like "call `get_weather` with `{city: "Seattle"}`." **Your code** runs the function and sends the result back. The model then continues.

```python
messages = [user_goal]
while True:
    reply = model(messages, tools=TOOLS)
    if reply.wants_tool:
        result = run_tool(reply.tool_name, reply.tool_args)   # YOUR code runs it
        messages += [reply, result]
    else:
        return reply.text
```

Ten lines, and you have an agent. Lesson 1 builds exactly this from scratch so there is no magic later.

**MCP (Model Context Protocol)** is an open standard for packaging tools so any agent can plug into them, a bit like USB for tools. We will use it once agents need lots of tools.

## 4. Key patterns you will meet

- **ReAct (Reason + Act):** the model alternates "thought → action → observation." The loop above is ReAct.
- **Plan-and-Execute:** first write a full plan, then carry it out step by step, re-planning when something fails.
- **Reflection / self-critique:** the agent reviews its own output and revises. Cheap and surprisingly effective.
- **RAG (Retrieval-Augmented Generation):** before answering, search a knowledge base and put the relevant passages in the prompt. This is how agents "know" your documents.
- **Routing:** a cheap classifier step sends each request to the right specialist.
- **Orchestrator–workers:** one agent breaks the job up and hands pieces to sub-agents.
- **Evaluator–optimizer:** one model produces, another grades, loop until it passes.
- **Human-in-the-loop:** the agent pauses for approval before risky actions.

## 5. Memory, in three flavors

1. **Working memory:** the conversation in the context window. Fast, but limited and wiped each session.
2. **Long-term memory:** facts saved to a file or database and loaded later ("Mike prefers Python").
3. **Semantic memory:** text turned into **embeddings** (vectors of numbers that capture meaning) and stored in a vector index, so you can search by meaning instead of keywords.

The context window is the agent's desk: everything it can see right now. Memory is the filing cabinet. A lot of agent engineering is deciding what goes on the desk.

## 6. Multi-agent systems

Sometimes one agent with 40 tools gets confused. Splitting the job helps: a researcher, a writer, and an editor, each with a focused prompt and toolset. Common shapes:

- **Pipeline:** A → B → C
- **Supervisor:** a boss agent delegates and merges
- **Debate / voting:** several agents answer, then argue or vote

More agents means more cost and more ways to fail, so add them only when one agent clearly struggles.

## 7. What makes an agent "production-ready"

The difference between a demo and something you would trust:

- **Evals:** a test set of tasks with checks, run on every change. You cannot improve what you do not measure.
- **Observability:** log every model call, tool call, and token count so you can see why it did something.
- **Guardrails:** validate tool inputs, restrict permissions, sandbox code execution, and watch for **prompt injection** (malicious instructions hidden in web pages or documents the agent reads).
- **Cost and latency control:** caching, smaller models for easy steps, step limits so loops cannot run forever.
- **Graceful failure:** retries, timeouts, and a clear "I couldn't do this" instead of a confident wrong answer.

## 8. The framework landscape (2026, in one breath)

- **Raw provider SDKs** (Anthropic, OpenAI, Google): the most transparent. Best for learning.
- **Agent SDKs from model providers** (Claude Agent SDK, OpenAI Agents SDK, Google ADK): batteries-included loops, tools, and sub-agents.
- **Graph/orchestration frameworks** (LangGraph, CrewAI, AutoGen, LlamaIndex, Pydantic AI): explicit state machines, multi-agent wiring, RAG helpers.
- **Local models** (Ollama, llama.cpp): free and private, smaller and less capable.

The course recommendation is to build the core loop by hand first, then introduce a framework once you know what it is doing for you.

## 9. Glossary

- **Token:** a chunk of text (~¾ of a word). Models are priced and limited by tokens.
- **Context window:** how many tokens the model can see at once.
- **System prompt:** standing instructions that frame every turn.
- **Temperature:** randomness dial. Low for reliable, high for creative.
- **Structured output:** forcing the model to reply in a JSON schema.
- **Embedding:** a vector representing the meaning of text, used for similarity search.
- **Vector store:** a database that searches embeddings.
- **Hallucination:** a fluent, confident, wrong answer.
- **Grounding:** tying answers to retrieved sources so they can be checked.
- **Prompt injection:** untrusted content trying to hijack the agent's instructions.
- **Eval:** an automated test of agent behavior.
- **Trace:** the full recorded history of one agent run.

## 10. Where to go next

1. Skim [CURRICULUM.md](CURRICULUM.md) to see how these ideas map to the 30 agents.
2. Lesson 0 sets up your environment; Lesson 1 builds the loop above in about 50 lines.
