# ☁️ Ciel Codebase Intelligence & Engineering Layer


> *The AI coding assistant with a sky-level view of your entire product.*

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)
[![Discord](https://img.shields.io/badge/Discord-Join%20Community-7289DA)](https://discord.gg/ciel-dev)
[![GitHub Stars](https://img.shields.io/github/stars/ciel-dev/ciel?style=social)](https://github.com/ciel-dev/ciel)

---

Most AI coding assistants see one file.
Most see one repo.
**Ciel sees your entire product.**

Ciel is an open-source AI coding assistant that plugs into your IDE and understands all your repositories at once — how your microservices communicate, how your APIs are structured, how data flows across your entire system. Ask it anything. It knows the full picture.

```
You:   "If I change the user schema in auth-service, what else breaks?"

Ciel:  "Three things are affected:
        1. profile-service reads `user.avatarUrl` in /profile-service/src/resolvers/user.ts:34
        2. notification-service expects `user.email` in /notification-service/src/consumers/user.consumer.ts:19  
        3. The shared DTO in /packages/common/src/types/user.dto.ts:8 will need updating first
           since both services import from it."
```

No more tab-switching across repos. No more "I don't know what that service does."
Just answers — with full context.

---

## ✨ What Makes Ciel Different

| Feature | Ciel | Copilot | Cursor | Cody |
|---|---|---|---|---|
| Current file context | ✅ | ✅ | ✅ | ✅ |
| Single repo context | ✅ | ✅ | ✅ | ✅ |
| **Cross-repo context** | ✅ | ❌ | ❌ | ⚠️ |
| **Architecture mapping** | ✅ | ❌ | ❌ | ❌ |
| **Service dependency graph** | ✅ | ❌ | ❌ | ❌ |
| Self-hostable | ✅ | ❌ | ❌ | ✅ |
| Open source | ✅ | ❌ | ❌ | ✅ |
| LLM agnostic | ✅ | ❌ | ⚠️ | ✅ |

---

## 🏗️ How It Works

```
┌──────────────────────────────────────────────┐
│              Your IDE                         │
│     VS Code · JetBrains · Neovim (LSP)       │
└────────────────────┬─────────────────────────┘
                     │ query + current context
                     ▼
┌──────────────────────────────────────────────┐
│              Ciel API Server                  │
└──────────┬───────────────────────────────────┘
           │
     ┌─────▼──────┐        ┌──────────────────┐
     │  Retrieval  │        │   LLM Gateway    │
     │  Engine     │───────▶│ Claude/GPT/Local │
     │             │        └──────────────────┘
     │ ┌─────────┐ │
     │ │ Qdrant  │ │        ┌──────────────────┐
     │ │ Vectors │ │        │     Indexer       │
     │ └─────────┘ │        │                  │
     │ ┌─────────┐ │        │ AST Parser        │
     │ │  Neo4j  │ │        │ Protocol Detector │
     │ │  Graph  │ │        │ Embedder          │
     │ └─────────┘ │        └────────┬─────────┘
     └─────────────┘                 │
                              ┌──────▼──────────┐
                              │  Your Repos      │
                              │ svc-a · svc-b    │
                              │ svc-c · shared   │
                              └─────────────────┘
```

1. **Indexer** crawls all your repos, parses code with Tree-sitter, detects how services talk to each other (REST, gRPC, Kafka, etc.), and stores everything in a vector + graph database
2. **Retrieval Engine** finds the most relevant context for your query using semantic search + graph traversal
3. **LLM Gateway** sends the retrieved context to your chosen LLM and streams the answer back to your IDE

---

## 🚀 Quick Start

### 1. Run Ciel locally

```bash
git clone https://github.com/ciel-dev/ciel
cd ciel
cp .env.example .env
# Add your LLM API key (Anthropic or OpenAI) to .env
docker compose up -d
```

### 2. Add your repositories

```bash
# Add repos one by one
ciel repos add https://github.com/your-org/service-a
ciel repos add https://github.com/your-org/service-b

# Or add your entire GitHub org at once
ciel org add --org your-org --token ghp_xxxx
```

Ciel will index everything. First index takes a few minutes depending on repo count.

### 3. Install the IDE plugin

- **VS Code**: Search `Ciel` in the Extensions marketplace → Install → Set server URL to `http://localhost:8080`
- **JetBrains**: Search `Ciel` in Plugins → Install → Set server URL

Start asking questions. Ciel now knows your entire product.

---

## 📦 Project Structure

```
ciel/
├── packages/
│   ├── core/              # Shared types, interfaces, config
│   ├── indexer/           # Repo ingestion, AST parsing, embedding pipeline
│   ├── retrieval/         # Vector search + graph traversal
│   ├── llm-gateway/       # Claude, GPT-4, Ollama integrations
│   └── ide-plugin/
│       ├── vscode/        # VS Code extension
│       └── jetbrains/     # JetBrains plugin (Kotlin)
├── apps/
│   └── api/               # Main REST API server
├── docs/                  # Architecture deep-dives
├── docker/                # Docker configs
└── scripts/               # Dev & deployment utilities
```

---

## 🛣️ Roadmap

### v0.1 — Working Prototype *(active)*
- [ ] Repo ingestion & embedding pipeline
- [ ] Qdrant vector store integration
- [ ] Basic semantic retrieval
- [ ] VS Code plugin with chat UI
- [ ] Claude + OpenAI support
- [ ] Docker Compose self-hosting

### v0.2 — Architecture Intelligence
- [ ] Tree-sitter AST parsing (TS, Python, Go, Java)
- [ ] REST/gRPC/Kafka inter-service detection
- [ ] Neo4j graph store + graph-augmented retrieval
- [ ] JetBrains plugin

### v0.3 — Always Fresh
- [ ] Webhook-driven incremental re-indexing
- [ ] Change impact analysis
- [ ] OpenAPI / proto file ingestion

### v0.4 — Ecosystem
- [ ] Ollama / local LLM support
- [ ] Neovim plugin (LSP)
- [ ] Multi-tenant & RBAC
- [ ] GitHub App for one-click setup

Full roadmap → [docs/ROADMAP.md](docs/ROADMAP.md)

---

## 🤝 Contributing

Ciel is community-built. We need help with language parsers, retrieval algorithms, IDE plugins, LLM integrations, and docs.

Read [CONTRIBUTING.md](CONTRIBUTING.md) → pick a [`good first issue`](https://github.com/ciel-dev/ciel/labels/good%20first%20issue) → ship it.

---

## 💬 Community

- [Discord](https://discord.gg/ciel-dev) — Talk to contributors, share ideas
- [GitHub Discussions](https://github.com/ciel-dev/ciel/discussions) — RFCs and longer ideas
- [X / Twitter](https://twitter.com/ciel_dev) — Updates

---

## 📄 License

MIT — free to use, modify, and self-host.

---

<p align="center">
  <b>Ciel</b> — French for <i>sky</i>. Because good tools give you a view from above.<br/><br/>
  Built with ❤️ by the open source community.<br/>
  Star ⭐ if you believe developers deserve better tools.
</p>
