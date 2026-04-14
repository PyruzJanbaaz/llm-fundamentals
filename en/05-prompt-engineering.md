# 05 — Prompt Engineering

> Tags: `#PromptEngineering` `#LLM` `#RAG` `#Agents` `#Templates`

## Introduction
Prompt engineering is the practice of designing inputs that guide an LLM toward producing accurate, useful, and reliable outputs.  
Although LLMs are powerful, they are also sensitive to how information is presented.  
A well‑crafted prompt can dramatically improve reasoning, reduce hallucinations, and produce more consistent results.

Prompt engineering is not about “tricking” the model — it is about communicating with it in a way that aligns with how it processes information.

| Prompt design goal | What it achieves |
| --- | --- |
| Clear intent | More relevant answers |
| Structured format | Predictable output shapes |
| Context and examples | Fewer hallucinations |
| Constraints | Safer and more reliable responses |

---

## Why Prompt Engineering Matters
LLMs do not understand language the way humans do. They operate on statistical patterns learned from massive datasets.

| What goes wrong | Prompt engineering fix |
| --- | --- |
| Vague prompts | Add clear intent and scope |
| Missing context | Provide background and examples |
| Unclear instructions | Define exact output format |
| Mixed tasks | Separate into single goals |
| No constraints | Add limits and safety rules |

Prompt engineering gives the model:

- clarity  
- constraints  
- goals  
- structure  
- examples  

These elements help the model reason more effectively.

---

## How LLMs Interpret Prompts
An LLM does not “read” a prompt — it **predicts the next token** based on the prompt, its training data, and its internal representation of meaning.

| Prompt factor | Why it matters |
| --- | --- |
| Order of information | Sets the model’s interpretation priority |
| Level of detail | Balances specificity vs. creativity |
| Examples | Teaches the desired output pattern |
| Constraints | Limits unsafe or irrelevant responses |
| Formatting | Encourages structured output |

Prompt engineering aligns the prompt with the model’s internal prediction process.

---

## Core Principles of Prompt Engineering

| Principle | Why it matters | Example |
| --- | --- | --- |
| Be Specific | Reduces ambiguity and narrows focus | Ask for a three-level Transformer explanation |
| Provide Context | Limits hallucination and makes answers grounded | Include audience, scope, and background |
| Define Output Format | Controls response structure | Request JSON, bullet lists, or tables |
| Use Step-by-Step Reasoning | Improves accuracy and traceability | Ask the model to think step by step |
| Use Examples | Teaches the model the pattern you want | Provide few-shot input/output pairs |

### **1. Be Specific**
Vague prompts produce vague answers.  
Specific prompts produce targeted, accurate outputs.

Bad:
Explain transformers.

Good:
Explain the Transformer architecture at three levels:

- Simple explanation
- Engineering explanation
- AI engineering explanation

---

### **2. Provide Context**
LLMs fill in missing information with assumptions. Providing context reduces hallucinations.

Bad:
Write a summary.

Good:
Write a 200‑word summary of the following article for a technical audience.

---

### **3. Define the Output Format**
LLMs follow structure extremely well.

Examples:

- Markdown  
- JSON  
- bullet points  
- step‑by‑step reasoning  
- tables  

Clear structure → clear output.

---

### **4. Use Step‑by‑Step Reasoning**
LLMs perform better when asked to think in steps.

Example:
Think step by step and explain your reasoning before giving the final answer.


This improves:

- accuracy  
- reasoning depth  
- reliability  

---

### **5. Use Examples (Few‑Shot Prompting)**
Examples teach the model the pattern you want.

Example:
Rewrite the text in a formal tone.

Example:
Input: "Hey, can you send me the report?"
Output: "Could you please provide the report?"

Now rewrite:
"Bro, send me the file."

### Prompt Template Example
Use a reusable template to keep prompts consistent.

```text
You are a helpful AI assistant.

Task:
{task_description}

Audience:
{audience}

Format:
{format}

Context:
{context}

Constraints:
{constraints}
```

---

## Advanced Prompting Techniques

| Technique | What it does | When to use it |
| --- | --- | --- |
| Chain-of-Thought (CoT) | Encourages step-by-step reasoning | Complex reasoning tasks |
| Self-Consistency | Generates multiple reasoning paths | High-stakes answers |
| ReAct (Reason + Act) | Mixes reasoning with tool actions | Agent workflows |
| Tree-of-Thought | Explores branching solutions | Hard planning problems |
| Role Prompting | Assigns a persona or role | Behavior shaping and domain style |

Example:
You are an AI system architect. Explain the design tradeoffs.

---

## Prompt Engineering for RAG
In RAG systems, prompts must:

- reference retrieved documents  
- avoid hallucinating beyond the provided context  
- instruct the model to cite sources  
- constrain the model to the retrieved information  

| RAG prompt requirement | Why it matters |
| --- | --- |
| Reference retrieved documents | Ensures answers are grounded |
| Avoid hallucination | Prevents made-up facts |
| Cite sources | Improves trust and traceability |
| Constrain to context | Keeps output relevant |

Example:

```text
Answer using ONLY the provided context. If the answer is not in the context, say "Not found."
```

---

## Prompt Engineering for Agents
Agents require prompts that define:

- goals  
- constraints  
- tools  
- memory  
- planning steps  

| Agent prompt component | Purpose |
| --- | --- |
| Goals | Defines the desired outcome |
| Constraints | Prevents unsafe or irrelevant actions |
| Tools | Specifies available APIs or actions |
| Memory | Provides short-term state |
| Planning steps | Guides multi-step execution |

Example:

```text
You are an AI agent. Break the task into steps, decide which tools to use, and execute the plan.
```

---

## Common Prompting Mistakes

- being vague  
- forgetting to define the audience  
- mixing multiple tasks in one prompt  
- not specifying format  
- assuming the model knows context  
- asking for too much in one step  
- not constraining hallucinations  

Prompt engineering is about reducing ambiguity.

---

## Summary
Prompt engineering is the interface between humans and LLMs.  
It improves:

- accuracy  
- reasoning  
- reliability  
- structure  
- safety  

A good prompt gives the model:

- context  
- clarity  
- constraints  
- structure  
- examples  

Prompt engineering is not optional — it is essential for building effective AI systems.
