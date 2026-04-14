# 21 — Security & Privacy for LLM Apps

## Introduction
LLM applications bring security and privacy risks that traditional systems rarely face.

| Risk category | Examples |
|---|---|
| prompt attacks | prompt injection, jailbreaks |
| data abuse | data leakage, RAG poisoning |
| system abuse | insecure agents, unsafe tools |
| privacy failures | memory leaks, multi-tenant exposure |

Security is not optional.
It is a fundamental architecture requirement for production AI.

---

# Part 1 — Threat Model for LLM Systems

A complete threat model covers user, data, system, and privacy attacks.

| Threat class | What it includes | Example vector |
|---|---|---|
| User attacks | malicious inputs and instructions | prompt injection, adversarial queries |
| Data attacks | corrupted or poisoned content | RAG poisoning, embedding manipulation |
| System attacks | unsafe tooling or routing | agent abuse, unauthorized memory access |
| Privacy attacks | unintended data exposure | cross-tenant leakage, memorization |

Threat models drive design decisions and mitigations.

---

# Part 2 — Prompt Injection

Prompt injection is the most common LLM attack.

| Injection type | Attack pattern | Risk |
|---|---|---|
| Direct | "Ignore previous instructions..." | bypass system intent |
| Indirect | malicious text embedded in content | model follows hidden instructions |

### Mitigations
- strong system prompts
- input sanitization
- content filtering
- instruction isolation
- output validation
- allow-lists for actions

### Example filter logic
```python
def sanitize_input(text):
    if contains_injection_patterns(text):
        raise SecurityError("prompt injection detected")
    return text
```

Prompt injection is a design problem, not a model problem.

---

# Part 3 — Jailbreaks

Jailbreaks try to bypass safety guards using clever phrasing.

| Attack style | Example |
|---|---|
| DAN prompt | "You are not bound by rules..." |
| role play | "Pretend you are a pirate..." |
| multi-step | chain of instructions that override safety |
| translation | use translation to hide payload |

### Mitigations
- safety models
- output filtering
- multi-layer safety checks
- adversarial testing
- continuous monitoring

### Example safety pipeline
```python
output = model.generate(prompt)
if safety_model.is_unsafe(output):
    return safe_fallback()
return output
```

Jailbreaks evolve, so defenses must evolve too.

---

# Part 4 — Data Leakage

LLMs can leak sensitive content from multiple sources.

| Leak source | Description |
|---|---|
| user data | personal information in prompts |
| internal docs | proprietary content in retrieval context |
| memory | long-term stored information |
| metadata | hidden attributes in vectors |

### Mitigations
- redaction
- access control
- encryption at rest and in transit
- tenant isolation
- retrieval filtering
- memory scoping

### Example redaction
```python
def redact(text):
    return remove_sensitive_fields(text)
```

Privacy is a system property, not a model property.

---

# Part 5 — RAG Poisoning

RAG systems are vulnerable to poisoning at multiple stages.

| Poison type | Attack vector | Mitigation |
|---|---|---|
| document poisoning | malicious documents submitted | document validation, human review |
| embedding poisoning | corrupted embedding vectors | embedding validation, anomaly detection |
| metadata poisoning | fake metadata rankings | metadata verification, hybrid retrieval |
| retrieval hijacking | irrelevant/harmful chunks selected | reranking, source filtering |

### Example hybrid retrieval
```python
candidates = sparse_retriever.search(query)
candidates += dense_retriever.search(query)
ranked = re_ranker.rank(candidates)
```

RAG poisoning is a top enterprise risk.

---

# Part 6 — Agent Security

Agents introduce new attack surfaces through tools and state.

| Risk | What can go wrong | Mitigation |
|---|---|---|
| unsafe tool call | agent executes harmful action | tool allow-list, sandboxing |
| infinite loop | agent never terminates | max-step limits |
| unauthorized action | agent accesses forbidden data | role-based policies |
| unsafe arguments | attacker injects bad tool inputs | argument validation |

### Example safe agent pattern
```python
if tool_name not in ALLOWED_TOOLS:
    raise SecurityError("tool not permitted")
args = validate_args(tool_name, raw_args)
result = sandboxed_call(tool_name, args)
```

Agents should be treated like untrusted code.

---

# Part 7 — Memory Security

Memory systems store sensitive personal and context data.

| Memory risk | Mitigation |
|---|---|
| unauthorized access | encryption, ACLs |
| stale or bad data | validation, refresh policies |
| cross-user leakage | per-user isolation |
| poisoning | provenance checks |

### Memory lifecycle example
```python
store_memory(user_id, data, expires_in=30_days)
memory = retrieve_memory(user_id)
if memory.is_stale():
    refresh_memory(user_id)
```

Memory must be secure by design.

---

# Part 8 — Multi-Tenant Isolation

Shared infrastructure requires strict tenant separation.

| Isolation failure | Example impact | Mitigation |
|---|---|---|
| cross-tenant retrieval | wrong tenant data returned | tenant-aware retrieval |
| shared cache leakage | cached results leak | tenant-scoped caching |
| shared prompt leakage | system prompt content exposed | strict prompt partitioning |

### Tenant-scoped retrieval example
```python
results = retrieve(query, tenant_id=tenant.id)
```

Isolation is essential for privacy and compliance.

---

# Part 9 — Secure Tooling

Tools called by agents are privileged operations.

| Tool risk | Example | Mitigation |
|---|---|---|
| command injection | shell execution via agent | sandboxing, allow-lists |
| insecure APIs | external service misuse | API validation, whitelisting |
| unsafe file access | reading/writing sensitive files | path restrictions |
| unrestricted network | exfiltration via HTTP | network allow-lists, rate limiting |

### Tool audit example
```python
log_tool_call(user_id, tool_name, args)
if tool_call_failed:
    record_audit_event("tool failure")
```

Treat tools as highly privileged components.

---

# Part 10 — Compliance & Governance

LLM systems must align with legal and enterprise controls.

| Requirement | What to enforce |
|---|---|
| GDPR | data minimization, consent, rights management |
| HIPAA | protected health information handling |
| SOC2 | security and availability controls |
| ISO 27001 | information security management |
| enterprise policy | internal approval, audit logging |

### Governance checklist
- data minimization
- auditability
- explainability
- retention policies
- consent management

Compliance is part of security.

---

# Summary
Security and privacy are essential for safe, reliable, and compliant LLM systems.

A secure AI system must defend against:
- prompt injection
- jailbreaks
- data leakage
- RAG poisoning
- agent abuse
- insecure memory
- multi-tenant failures

Security is not a feature.
It is a core architectural requirement.

- HIPAA  
- SOC2  
- ISO 27001  
- internal enterprise policies  

Key requirements:
- data minimization  
- auditability  
- explainability  
- retention policies  
- user consent  

Compliance is part of security.

---

# Summary
Security and privacy are essential for building safe, reliable, and compliant LLM systems.  
A secure AI system must defend against:

- prompt injection  
- jailbreaks  
- data leakage  
- RAG poisoning  
- agent abuse  
- insecure memory  
- multi‑tenant failures  

Security is not a feature —  
it is a fundamental architectural requirement.
