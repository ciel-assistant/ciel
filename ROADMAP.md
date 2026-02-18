# Ciel Roadmap

This is a living document. Priorities shift as the community grows.
Open a [Discussion](https://github.com/ciel-dev/ciel/discussions) to propose changes or reprioritize.

---

## v0.1 — Working Prototype *(current focus)*
*Goal: End-to-end flow working. Index a repo, ask a question, get an answer in the IDE.*

- [ ] Repo ingestion pipeline (clone, chunk, embed)
- [ ] Qdrant vector store integration
- [ ] Basic semantic retrieval (vector search only)
- [ ] FastAPI backend with streaming
- [ ] VS Code extension with chat UI
- [ ] Claude + OpenAI LLM support
- [ ] Docker Compose for self-hosting
- [ ] `.env.example` and setup documentation

---

## v0.2 — Architecture Intelligence
*Goal: Ciel understands services, not just files.*

- [ ] Tree-sitter AST parsing (TypeScript, Python, Go, Java)
- [ ] REST endpoint & outbound call detection
- [ ] gRPC proto file parsing
- [ ] Kafka/RabbitMQ topic detection
- [ ] Neo4j graph store integration
- [ ] Graph-augmented retrieval (vector + graph hybrid)
- [ ] JetBrains plugin (IntelliJ, PyCharm, GoLand)
- [ ] GitHub org bulk import

---

## v0.3 — Always Fresh
*Goal: Index stays current with zero manual effort.*

- [ ] GitHub/GitLab webhook-driven incremental re-indexing
- [ ] OpenAPI / Swagger spec ingestion
- [ ] Proto file contract mapping
- [ ] Change impact analysis ("what breaks if I change X?")
- [ ] Improved semantic chunking (function-boundary aware)
- [ ] Context window optimizer & re-ranking

---

## v0.4 — Ecosystem & Scale
*Goal: Works for large orgs. Works everywhere.*

- [ ] Ollama / local LLM support
- [ ] Gemini, Mistral, Cohere integrations
- [ ] Neovim plugin (LSP server)
- [ ] GitHub App for one-click repo connection
- [ ] Multi-tenant support
- [ ] Role-based repo access control
- [ ] Usage analytics dashboard (self-hosted)

---

## Future Ideas *(community input welcome)*

- Automatic PR review with cross-repo context awareness
- "Explain this architecture" — auto-generate architecture diagrams
- Slack / Teams bot interface
- Automatic runbook generation from code
- Test coverage gap detection across services
- Fine-tuning pipeline on your own codebase

---

Vote on priorities or suggest new ones in [GitHub Discussions](https://github.com/ciel-dev/ciel/discussions).
