# Contributing to Ciel

Thank you for being here. Ciel is being built in the open because the best developer tools should be made *by* developers, *for* developers. Every contribution — big or small — makes this better for everyone.

---

## 🧭 Before You Start

- Join our [Discord](https://discord.gg/ciel-dev) and say hi in `#contributors`
- Browse [open issues](https://github.com/ciel-dev/ciel/issues)
- Look for [`good first issue`](https://github.com/ciel-dev/ciel/labels/good%20first%20issue) if you're new
- For large changes, open a [Discussion](https://github.com/ciel-dev/ciel/discussions) first before writing code

---

## 🛠️ Local Development Setup

### Prerequisites

- Node.js 20+
- Python 3.11+
- Docker & Docker Compose
- Git

### Setup

```bash
# 1. Fork and clone
git clone https://github.com/YOUR_USERNAME/ciel
cd ciel

# 2. Install dependencies
npm install
pip install -r requirements.txt

# 3. Configure environment
cp .env.example .env
# Fill in at minimum: ANTHROPIC_API_KEY or OPENAI_API_KEY

# 4. Start infrastructure (Qdrant, Neo4j, Redis)
docker compose -f docker/docker-compose.dev.yml up -d

# 5. Start the API
npm run dev:api

# 6. Start the indexer
npm run dev:indexer
```

---

## 📁 Where Things Live

| What you want to work on | Where to look |
|---|---|
| Core types & shared utilities | `packages/core/` |
| Repo ingestion & code parsing | `packages/indexer/` |
| Search & context retrieval | `packages/retrieval/` |
| LLM provider integrations | `packages/llm-gateway/` |
| VS Code extension | `packages/ide-plugin/vscode/` |
| JetBrains plugin | `packages/ide-plugin/jetbrains/` |
| REST API server | `apps/api/` |

---

## 🔄 How to Contribute

```bash
# 1. Create a branch
git checkout -b feat/your-feature

# 2. Make your changes

# 3. Run tests
npm test
pytest packages/

# 4. Lint
npm run lint
ruff check .

# 5. Commit (follow Conventional Commits)
git commit -m "feat: add rust language parser"

# 6. Push and open a PR against main
```

Fill out the PR template fully. A maintainer will review within a few days.

---

## 💡 Where We Need Help Most

### 🔍 Indexer / Parsing
- Tree-sitter parsers for new languages (Rust, C#, Ruby, PHP...)
- Better detection of gRPC calls from generated stubs
- Parsing Kubernetes manifests to map network topology
- Terraform / infra-as-code parsing

### 🔗 Retrieval
- Hybrid BM25 + vector search
- Better graph traversal heuristics
- Context window optimizer / re-ranking layer
- Benchmarking retrieval quality

### 🤖 LLM Gateway
- Gemini, Mistral, Cohere integrations
- Ollama support for fully local inference
- Streaming improvements

### 🔌 IDE Plugins
- Neovim plugin (LSP-based)
- Eclipse plugin
- Better VS Code inline suggestions
- JetBrains UI polish

### 🧪 Tests & Quality
- Integration tests for the indexer pipeline
- Eval harness for answer quality
- More language parser edge case tests

### 📖 Docs
- Setup guides for different environments
- "How Ciel works" explainers
- Video walkthroughs

---

## 🧹 Code Style

- **TypeScript**: ESLint + Prettier — run `npm run lint`
- **Python**: Ruff — run `ruff check .`
- **Commits**: [Conventional Commits](https://www.conventionalcommits.org/)
  - `feat:` new feature
  - `fix:` bug fix
  - `docs:` documentation only
  - `refactor:` no behavior change
  - `test:` adding tests

---

## 🚫 What We Won't Accept

- Telemetry or phone-home behavior without explicit opt-in
- GPL/AGPL dependencies in core packages (MIT and Apache 2.0 are fine)
- Breaking changes without a migration path

---

## 🙋 Getting Help

- `#dev-help` on Discord
- Comment on the issue you're working on
- Open a GitHub Discussion

We're friendly. No question is too small.

---

## 🏆 Recognition

All contributors are listed in [CONTRIBUTORS.md](CONTRIBUTORS.md).
Consistent contributors may be invited to become maintainers.

Thank you. 🙌
