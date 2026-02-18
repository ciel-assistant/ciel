# Ciel — Architecture Deep Dive

This document explains how Ciel works internally. Read this before contributing to any core package.

---

## The Big Picture

Ciel is a **retrieval-augmented generation (RAG) system specialized for multi-repo codebases**.

The core problem it solves: LLMs have limited context windows, and your product likely spans many repos with millions of lines of code. You can't feed all of that to an LLM. So Ciel's job is to **find exactly the right pieces of context** for any given question — across all your repos — and hand those to the LLM.

The system has four layers:

```
Indexing → Storage → Retrieval → Generation
```

---

## Layer 1: Indexing

When you add a repo, the indexer runs a pipeline:

### Step 1 — Clone & Walk
The repo is cloned (or pulled if already cached). The file tree is walked, respecting `.gitignore`. Non-code files (images, lock files, binaries) are skipped.

### Step 2 — AST Parsing (Tree-sitter)
Each source file is parsed into an Abstract Syntax Tree using [Tree-sitter](https://tree-sitter.github.io/). We extract:
- Function and class definitions with signatures
- Import statements (what does this file depend on?)
- Exported types and interfaces
- HTTP route definitions (Express, FastAPI, Gin, Spring, Django...)

### Step 3 — Protocol Detection
Specialized detectors scan for cross-service communication:
- **REST**: Outbound HTTP calls (fetch, axios, requests, http.Get...) — we extract the target URL pattern
- **gRPC**: `.proto` file parsing + detection of generated stub usage
- **Events**: Kafka topic produce/consume calls, RabbitMQ exchange declarations, SNS/SQS usage
- **Shared packages**: Internal npm/pip packages imported across repos

### Step 4 — Chunking
Files are split into semantically meaningful chunks. Functions become individual chunks. Related functions are grouped. Each chunk carries metadata: file path, repo, line range, parent class, imports.

### Step 5 — Embedding
Each chunk is embedded using a code-optimized model (default: Voyage Code 2). The embedding captures meaning, not just keywords.

### Step 6 — Graph Building
While embedding, the indexer also writes edges to Neo4j:
```
FILE        -[IMPORTS]->          FILE
SERVICE     -[CALLS_REST]->       SERVICE {endpoint}
SERVICE     -[PUBLISHES_EVENT]->  TOPIC
SERVICE     -[SUBSCRIBES_EVENT]-> TOPIC
FUNCTION    -[DEFINED_IN]->       FILE
PACKAGE     -[USED_BY]->          SERVICE
```

---

## Layer 2: Storage

Two complementary stores work together:

### Qdrant (Vector Store)
Stores chunk embeddings. Used for **semantic search** — finding code that means something similar to the query, even if it uses different words.

### Neo4j (Graph Store)
Stores structural relationships. Used for **traversal** — "given service A is relevant, pull in everything it depends on or that depends on it."

The combination is what makes Ciel powerful. Vector search finds the entry point. Graph traversal pulls in structurally related context that vector search alone would miss.

---

## Layer 3: Retrieval

When a query arrives from the IDE:

1. **Embed the query** using the same model as indexing
2. **Vector search** — find top-K semantically similar chunks
3. **Identify services/files** those chunks belong to
4. **Graph traverse** — from those nodes, pull in: connected services, API contracts, event producers/consumers, shared packages
5. **Rank & trim** — score everything, trim to fit the LLM context window
6. **Format** — annotate with file paths, line numbers, relationship descriptions

The retrieval engine is the most critical component. A bad retrieval = a bad answer regardless of LLM quality.

---

## Layer 4: Generation

The assembled context + query are sent to the LLM. The prompt includes:
- System prompt (Ciel's role, product being analyzed)
- Retrieved code chunks with file paths and line numbers
- Relationship context ("service-a calls service-b via REST at /api/checkout")
- The user's currently open file (local context from IDE)
- The user's question

The response is streamed back to the IDE plugin.

---

## Keeping the Index Fresh

Two sync modes:
- **Webhook mode** (recommended): GitHub/GitLab/Bitbucket webhooks trigger incremental re-indexing on push. Only changed files are re-processed.
- **Polling mode**: Indexer checks for new commits on a configurable interval.

---

## Security

- Repo credentials stored encrypted at rest
- Code stays in your infrastructure — only query + retrieved chunks go to the LLM provider
- Multi-tenant deployments use namespace isolation in Qdrant and Neo4j
- All API calls authenticated with a secret token

---

## Further Reading

- [Retrieval Strategies](./retrieval-strategies.md)
- [Adding a Language Parser](./adding-language-support.md)
- [LLM Gateway](./llm-gateway.md)
- [IDE Plugin Architecture](./ide-plugin-architecture.md)
- [Roadmap](./ROADMAP.md)
