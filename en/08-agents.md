# 08 — Agents

> Tags: `#Agents` `#Tools` `#Memory` `#Automation` `#LLM`

## Introduction
An LLM on its own is a text prediction engine.  
An **Agent** is an LLM connected to tools, memory, and an environment, allowing it to take actions, observe results, and iterate toward a goal.

Agents transform LLMs from passive text generators into **active problem‑solvers** capable of:

- searching the web  
- running code  
- querying databases  
- controlling software  
- orchestrating workflows  
- making multi‑step plans  

Agents are the foundation of modern AI systems that go beyond chat.

| Agent capability | Why it matters |
| --- | --- |
| Tool usage | Accesses external systems and real-time data |
| Memory | Maintains state across interactions |
| Planning | Breaks complex tasks into steps |
| Observation | Uses results to revise behavior |

---

## What Is an Agent?
An agent is an LLM wrapped in a loop:

1. **Observe** the environment  
2. **Think** about what to do  
3. **Act** using tools  
4. **Reflect** on the result  
5. **Repeat** until the goal is achieved  

This loop is what gives agents autonomy and persistence.

An LLM alone cannot do this.  
It can only generate text.  
An agent can **take actions**.

---

## Why Agents Exist
LLMs have limitations that prevent them from operating in real systems.

| Limitation | Agent solution |
| --- | --- |
| No real-time data access | Connects to external APIs and services |
| Cannot run code | Executes code through tool calls |
| Cannot browse the web | Uses web search tools |
| No long-term memory | Stores state in memory systems |
| No external interaction | Sends actions to systems and workflows |

Agents solve these limitations by connecting the LLM to tools, APIs, databases, vector stores, operating systems, and planning modules.

Agents turn LLMs into **general-purpose automation engines**.

---

## The Agent Loop (Core Architecture)
A modern agent follows a structured loop.

| Stage | What happens | Example |
| --- | --- | --- |
| **Observation** | Collects user input, tool results, memory, and environment state | Read API response and previous steps |
| **Reasoning** | Decides next action, missing information, and task breakdown | Choose search or code execution |
| **Action** | Executes a tool call, query, search, or workflow step | Call web search or run a script |
| **Reflection** | Evaluates success and decides if more steps are needed | Check results and revise plan |
| **Loop** | Repeats until the goal is complete | Continue until task done |

Example pseudocode:

```python
while not task.complete():
    observation = agent.observe()
    plan = llm.reason(observation)
    result = agent.act(plan)
    agent.reflect(result)
```

This loop is the heart of agentic behavior.

---

## ReAct: Reason + Act
One of the most influential agent frameworks is **ReAct**, which combines:

- **Chain‑of‑Thought reasoning**  
- **Tool usage**  

ReAct prompts the model to:

1. Think step by step  
2. Decide on an action  
3. Execute the action  
4. Observe the result  
5. Continue reasoning  

This creates a powerful feedback loop.

---

## Tools: Extending the Agent’s Capabilities
Tools are functions the agent can call.

| Tool type | Purpose | Example |
| --- | --- | --- |
| Web search | Find current or external information | Search engine API |
| Code execution | Run and validate code | Python REPL, notebook |
| SQL queries | Query structured data | Database access |
| Vector retrieval | Find semantically similar content | Embedding search |
| API calls | Interact with services | CRM, calendars, email |
| File operations | Read/write documents | File system tools |
| Calculators | Perform numeric computation | Math tools |
| Domain-specific tools | Solve specialized tasks | Diagnostics, finance, legal |

Tools turn the agent into a **programmable intelligence layer**.

---

## Memory: Beyond the Context Window
Agents need memory to operate over long tasks.

| Memory type | Storage | Purpose |
| --- | --- | --- |
| **Short-Term Memory** | Context window | Holds immediate task state |
| **Long-Term Memory** | Vector DB, key-value store, knowledge graph | Persists facts and state across sessions |
| **Episodic Memory** | Logs, transcripts | Records past interactions |
| **Semantic Memory** | Fact database | Stores concepts and relationships |

Memory allows agents to:

- remember goals  
- track progress  
- store results  
- maintain state across steps  

---

## Planning: Multi‑Step Reasoning
Agents must break tasks into steps.

Planning strategies include:

- Chain‑of‑Thought  
- Tree‑of‑Thought  
- Graph‑of‑Thought  
- Self‑Reflection  
- Self‑Critique  
- Iterative refinement  

Planning is what allows agents to solve:

- multi‑step workflows  
- research tasks  
- data analysis  
- software automation  
- enterprise processes  

---

## Agents vs RAG vs LLMs
| System | What it does | Key capability |
| --- | --- | --- |
| **LLM** | Predicts text | Pure language modeling |
| **RAG** | Retrieves knowledge and grounds answers | External knowledge integration |
| **Agent** | Takes actions, plans, and interacts | Tool usage and memory |

| System | Limitations |
| --- | --- |
| LLM | No tools, no memory, no actions |
| RAG | Grounded answers but no actions |
| Agent | Complex to build but highly capable |

Agents = LLM + Tools + Memory + Planning + Environment

---

## Agent Architectures (Engineering View)

A production‑grade agent system includes:

| Component | Responsibility |
| --- | --- |
| **LLM Core** | Produces reasoning and plans |
| **Tool Router** | Maps decisions to tool calls |
| **Planner** | Breaks tasks into steps |
| **Memory System** | Stores short and long-term state |
| **State Manager** | Tracks environment and task status |
| **Execution Engine** | Runs tool calls and workflows |
| **Observation Handler** | Captures and normalizes results |
| **Guardrails** | Enforces safety and constraints |
| **Logging & Monitoring** | Audits behavior and performance |

This is a full software architecture, not just a prompt.

---

## Agents in Enterprise Systems
Agents are used for:

- customer support automation  
- IT troubleshooting  
- workflow orchestration  
- financial analysis  
- legal document processing  
- automotive diagnostics  
- software development automation  
- data extraction and transformation  
- multi‑step business processes  

Agents are the backbone of next‑generation enterprise AI.

---

## Challenges in Agent Systems
Agents are powerful but difficult to engineer.

Challenges include:

- tool misuse  
- infinite loops  
- hallucinated actions  
- incorrect planning  
- missing context  
- memory drift  
- cost and latency  
- safety and guardrails  

Building reliable agents requires careful system design.

---

## Summary
Agents transform LLMs into autonomous problem‑solvers.  
They combine:

- reasoning  
- planning  
- tool usage  
- memory  
- environment interaction  

Agents are not just a feature —  
they are the future of AI systems and automation.

They turn LLMs from text generators into **intelligent actors** capable of real‑world impact.
