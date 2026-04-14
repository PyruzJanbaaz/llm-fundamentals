# 23 — Multi-Agent Systems

## Introduction
Multi-agent systems turn a single LLM into a team of collaborating specialists.
They enable agents to:

- collaborate
- coordinate
- negotiate
- specialize
- share knowledge
- divide tasks
- validate each other

A multi-agent system behaves more like a **team of experts** than a single model.

This chapter explains how to design, orchestrate, and optimize multi-agent architectures.

---

# Part 1 — Why Multi-Agent Systems?

| Single-agent limitation | Multi-agent solution |
|---|---|
| limited working memory | distributed memory and state |
| shallow reasoning depth | multi-step analysis across agents |
| weak specialization | domain-specific agents |
| hallucination risk | cross-verification and critics |
| hard long-horizon tasks | decomposition into subtasks |

Multi-agent systems are essential for advanced automation.

---

# Part 2 — Types of Agents

| Agent type | Role | Example |
|---|---|---|
| Specialist | handles one domain | legal, finance, coding |
| Planner | breaks tasks into subtasks | workflow manager |
| Critic | evaluates outputs | correctness, grounding, safety |
| Tool-using | calls external services | API, DB, calculator |
| Memory | manages long-term state | user prefs, session context |
| Supervisor | resolves conflicts and finalizes decisions | system overseer |

### Example agent definitions
```python
agents = {
    "planner": PlannerAgent(),
    "researcher": SpecialistAgent(domain="medical"),
    "critic": CriticAgent(),
}
```

---

# Part 3 — Multi-Agent Architectures

| Architecture | Pattern | Best for |
|---|---|---|
| Sequential | pipeline of agents | structured workflows |
| Parallel | simultaneous agents | brainstorming, multiple views |
| Hierarchical | supervisor → managers → workers | complex enterprise tasks |
| Negotiation | agents debate to reach consensus | decision making, optimization |
| Swarm | many lightweight agents | search, exploration, simulation |

### Example sequential flow
```python
plan = planner.create_plan(task)
research = researcher.run(plan)
draft = writer.generate(research)
final = critic.review(draft)
```

---

# Part 4 — Communication Between Agents

Agents must exchange information reliably.

| Channel | Description | Use case |
|---|---|---|
| messages | structured or natural language | task handoff |
| shared memory | vector DB or key-value store | intermediate state |
| blackboards | shared workspace | collaborative planning |
| tool calls | pass data through external tools | database lookups |

### Example message exchange
```python
message = {
    "from": "planner",
    "to": "researcher",
    "task": "find references for topic",
}
```

Communication should be structured, auditable, and rate-limited.

---

# Part 5 — Coordination Strategies

| Strategy | What it does |
|---|---|
| Task decomposition | breaks problems into smaller tasks |
| Role assignment | gives each agent a clear responsibility |
| Turn-taking | controls message order and prevents chaos |
| Voting | selects the best answer from multiple agents |
| Debate | refines reasoning through argument |
| Arbitration | supervisor resolves conflicts |

### Coordination example
```python
if agents_agree(results):
    return consensus_answer(results)
else:
    return supervisor.resolve(results)
```

Coordination is the core of multi-agent design.

---

# Part 6 — Reducing Hallucinations with Multi-Agents

Multi-agent systems reduce hallucinations by adding checks and grounding.

| Technique | Benefit |
|---|---|
| cross-verification | multiple agents validate answers |
| critic agents | dedicated error detection |
| retrieval agents | ensure grounded context |
| safety agents | filter unsafe content |
| consensus | require agreement before final output |

### Verification example
```python
candidate = worker.generate(task)
if critic.evaluate(candidate) < threshold:
    candidate = worker.regenerate(task)
```

More agents often means safer output.

---

# Part 7 — Multi-Agent Failure Modes

| Failure | Cause | Mitigation |
|---|---|---|
| infinite loops | agents keep delegating | step limits, supervisor intervention |
| conflicting agents | disagreement without resolution | arbitration and voting |
| over-coordination | too many messages | simplify workflow |
| under-coordination | agents ignore each other | enforce communication rules |
| tool abuse | unnecessary or unsafe calls | tool allow-lists, validation |
| memory corruption | incorrect shared state | memory validation, versioning |

### Failure mitigation example
```python
if agent.steps > MAX_STEPS:
    supervisor.terminate(agent)
```

Mitigation requires supervision, constraints, and observability.

---

# Part 8 — Scaling Multi-Agent Systems

Scaling multi-agent systems safely requires:

| Area | Approach |
|---|---|
| execution | parallelize where possible |
| caching | reuse intermediate results |
| batching | combine requests for efficiency |
| routing | route tasks to appropriate agent types |
| memory | isolate per-agent and per-user memory |
| cost | monitor usage and set budget limits |
| agent limits | cap steps and invocations |

### Scaling example
```python
with ThreadPoolExecutor() as pool:
    futures = [pool.submit(agent.run, task) for agent in agents]
```

Without optimization, multi-agent systems can become expensive quickly.

---

# Part 9 — Best Practices

| Practice | Why it matters |
|---|---|
| keep agents simple | reduces complexity and bugs |
| use a supervisor | prevents uncontrolled behavior |
| limit agent steps | avoids runaway loops |
| use shared memory carefully | avoids corruption |
| add critics | improves quality |
| log everything | simplifies debugging |

### Best practice example
```python
log_event("agent_step", agent=agent.name, step=step)
```

Debugging multi-agent systems is hard, so observability is critical.

---

# Summary
Multi-agent systems transform LLMs into coordinated teams of intelligent agents.
They enable:

- specialization
- collaboration
- verification
- complex planning
- long-horizon tasks
- safer and more reliable outputs

Well-designed multi-agent architectures are essential for advanced AI and enterprise automation.
