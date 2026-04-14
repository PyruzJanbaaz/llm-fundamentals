# 02 — Tokenization

> Tags: `#Tokenization` `#LLM` `#Embeddings` `#Context`

## Introduction
Before a language model can understand or generate text, it must convert raw text into a form it can process. Computers do not understand words, sentences, or grammar — they understand numbers. Tokenization is the process that transforms human language into numerical units called **tokens**, which the model can embed, analyze, and predict.

Tokenization is the first step in every LLM pipeline. Without it, no attention, no embeddings, and no reasoning can happen.

| Role | Why it matters |
| --- | --- |
| Text → tokens | Converts human language into model-readable input |
| Vocabulary control | Reduces model size and handles rare words |
| Context efficiency | Determines how much text fits into the window |


## What Is a Token?
A token is the smallest unit of text that the model processes.  
A token is **not** the same as a word.

| Token type | Example |
| --- | --- |
| Full word | `apple` |
| Subword | `un` + `believ` + `able` |
| Prefix / suffix | `pre`, `ing`, `ly` |
| Character | `a`, `b`, `c` |
| Punctuation | `.`, `,`, `?` |
| Whitespace | ` ` |

Example:

```python
text = "unbelievable"
tokens = tokenizer.tokenize(text)
print(tokens)
# ['un', 'believ', 'able']
```

This allows the model to handle new or rare words by breaking them into familiar components.


## Why Tokenization Exists
Human language is messy:

| Challenge | Tokenization benefit |
| --- | --- |
| Variable word length | Breaks words into stable subword units |
| Multiple scripts | Supports multilingual and byte-level text |
| New words | Creates unseen text from familiar pieces |
| Rich morphology | Reuses word parts instead of whole words |
| Inconsistent punctuation | Normalizes boundaries across input |

If a model tried to learn every word as a unique symbol, it would need millions of entries and still fail on unseen words.

Tokenization solves this by:

| Outcome | Why it matters |
| --- | --- |
| Reducing vocabulary size | Smaller models and faster lookup |
| Enabling generalization | Handles new words with learned parts |
| Managing rare words | Shares representations across tokens |
| Capturing word structure | Preserves meaning in subword pieces |

Tokenization is the bridge between human language and machine computation.


## How Tokenization Works
Modern LLMs use subword tokenization algorithms. The most common ones are:

| Algorithm | Used by | Characteristics |
| --- | --- | --- |
| **Byte Pair Encoding (BPE)** | GPT-2, GPT-3 | Merges frequent byte pairs into subwords |
| **WordPiece** | BERT | Builds vocabulary via likelihood maximization |
| **SentencePiece** | LLaMA, PaLM | Language-agnostic, works on raw bytes |

These algorithms aim to balance:

- vocabulary size  
- expressiveness  
- efficiency  
- ability to handle any text  

```python
text = "I love NLP."
ids = tokenizer.encode(text)
print(ids)
# [40, 120, 321, 13]
embeddings = model.embed(ids)
```

## Tokens and Embeddings
Once text is tokenized, each token is mapped to a numerical vector called an **embedding**.  
Tokenization defines:

- how many embeddings the model needs  
- how long sequences become  
- how much memory is required  
- how much computation is needed  

Tokenization directly affects:

- inference cost  
- context window usage  
- chunking in RAG  
- prompt design  

---

## Tokenization and Context Windows
LLMs operate within a fixed context window (e.g., 8k, 32k, 128k tokens). Tokenization determines how quickly that window fills.

Example:

```text
This is a sentence.
```

may be 5 tokens in one tokenizer but 12 tokens in another.

| Tokenizer style | Context cost |
| --- | --- |
| Coarse word-based | Fewer tokens, less flexibility |
| Subword-based | More tokens, better generalization |

Better tokenization means more efficient use of the model’s context window.


## Why Tokenization Matters for AI Engineers
Tokenization affects every part of an AI system:

- **RAG** → chunk sizes depend on tokens  
- **Agents** → planning and memory depend on tokens  
- **Prompting** → cost and performance depend on tokens  
- **Latency** → more tokens = slower inference  
- **Cost** → more tokens = higher compute cost  
- **Evaluation** → token‑level metrics matter  

Understanding tokenization is essential for building efficient, scalable LLM systems.

## Summary
Tokenization is the foundation of all LLM behavior.  
It converts text into tokens, enabling:

- embeddings  
- attention  
- reasoning  
- generation  

Without tokenization, LLMs cannot operate.  
It is the first and most fundamental step in the entire language modeling pipeline.
