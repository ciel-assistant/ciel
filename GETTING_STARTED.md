# 🚀 Getting Started — Launching Ciel as an Open Source Project

This guide walks you through every step to go from zero to a live, open source GitHub project that other developers can discover, use, and contribute to.

---

## Phase 1: GitHub Setup (Day 1)

### Step 1 — Create a GitHub Account / Org
1. Go to [github.com](https://github.com) and sign in
2. Click your profile picture → **Your organizations** → **New organization**
3. Choose the **Free** plan
4. Name it `ciel-dev`
5. This gives you the URL: `github.com/ciel-dev`

### Step 2 — Create the Repository
1. Inside the `ciel-dev` org, click **New repository**
2. Name: `ciel`
3. Description: `The AI coding assistant with a sky-level view of your entire product`
4. Set to **Public**
5. Do NOT initialize with README (you already have one)
6. Click **Create repository**

### Step 3 — Push This Scaffold
```bash
# In the ciel/ folder from this download
git init
git add .
git commit -m "feat: initial project scaffold"
git branch -M main
git remote add origin https://github.com/ciel-dev/ciel.git
git push -u origin main
```

### Step 4 — Configure the Repo on GitHub
Go to your repo → **Settings** and do the following:

- **General** → Add a website URL (even a placeholder like `https://ciel-dev.github.io`)
- **General** → Add topics: `ai`, `coding-assistant`, `developer-tools`, `llm`, `open-source`, `ide-plugin`, `rag`
- **Features** → Enable **Discussions** (critical for community)
- **Branches** → Add a branch protection rule on `main`: require PR reviews before merging

---

## Phase 2: Community Infrastructure (Day 1-2)

### Step 5 — Create a Discord Server
1. Open Discord → click **+** → **Create My Own** → **For a club or community**
2. Name it `Ciel`
3. Create these channels:
   - `#announcements` (read-only)
   - `#general`
   - `#contributors`
   - `#dev-help`
   - `#ideas`
   - `#showcase`
4. Get the invite link and update it in your README and CONTRIBUTING.md

### Step 6 — Create Seed Issues
Great open source projects have clear, welcoming issues. Create 8-10 issues right away:

**Good first issues (small, well-defined):**
- "Add Rust language parser (Tree-sitter)"
- "Add Gemini LLM provider to llm-gateway"
- "Write unit tests for the chunker module"
- "Add dark mode to VS Code plugin chat UI"
- "Write setup guide for Windows users"

**Help wanted (medium complexity):**
- "Implement BM25 + vector hybrid search"
- "Build gRPC proto file parser"
- "Add Ollama support for local inference"
- "Create Neovim plugin (LSP-based)"

Label them properly: `good first issue`, `help wanted`, `documentation`, etc.

### Step 7 — Set Up GitHub Discussions
Go to **Discussions** tab → Create starter threads:
- 📣 "Welcome to Ciel — introduce yourself!"
- 🗺️ "Roadmap discussion — what should we prioritize?"
- 💡 "Ideas & feature requests"

---

## Phase 3: First Working Code (Week 1-2)

### Step 8 — Build the v0.1 Core (your first milestone)

Start with the simplest possible end-to-end flow:

```
Single repo → index → ask question → get answer in terminal
```

Work in this order:

1. **`packages/core`** — Define shared TypeScript types: `Chunk`, `Repo`, `Query`, `RetrievalResult`
2. **`packages/indexer`** — Build a basic pipeline: clone repo → read files → split into chunks → embed with OpenAI → store in Qdrant
3. **`packages/retrieval`** — Basic vector search: embed query → search Qdrant → return top chunks
4. **`apps/api`** — Simple FastAPI or Express endpoint: POST `/query` → run retrieval → call LLM → stream response
5. **`packages/ide-plugin/vscode`** — Minimal VS Code extension: chat panel → calls your API → shows streaming response

Get this working locally first. Then wire up Docker Compose so anyone can run it with one command.

### Step 9 — Record a Demo
Once v0.1 works, record a 2-minute screen recording showing:
- Ciel answering a real cross-repo question
- The answer being accurate and citing the right file/line

Upload to YouTube and link it in the README. This is your #1 recruiting tool for contributors.

---

## Phase 4: Launch (Week 2-3)

### Step 10 — Launch on Hacker News
Post to [news.ycombinator.com](https://news.ycombinator.com) → Submit:
- Title: `Ciel – Open-source AI coding assistant that understands your entire product (not just one file)`
- Post on a weekday morning (9-10am EST) for maximum visibility
- Be in the comments all day to respond

### Step 11 — Post on Reddit
- [r/programming](https://reddit.com/r/programming)
- [r/LocalLLaMA](https://reddit.com/r/LocalLLaMA) (for the self-hosting angle)
- [r/MachineLearning](https://reddit.com/r/MachineLearning)
- [r/webdev](https://reddit.com/r/webdev)

### Step 12 — Share on X / Twitter
Create `@ciel_dev` and post:
```
Introducing Ciel ☁️ — open source AI coding assistant that knows your ENTIRE product.

Not just your current file. Not just one repo.
All your microservices. All their connections.

Ask it anything across your entire codebase.

github.com/ciel-dev/ciel

#opensource #devtools #AI
```

### Step 13 — Share in Developer Communities
- Post in relevant Discord servers (e.g. Theo's T3 Discord, Fireship Discord, The Primeagen's community)
- Share in relevant Slack communities
- Write a dev.to or Hashnode blog post explaining the architecture

---

## Phase 5: Keep the Momentum (Ongoing)

- Merge PRs within 48 hours to keep contributors engaged
- Post weekly updates in `#announcements` on Discord
- Tweet progress updates — even small wins ("Just merged gRPC detection 🎉")
- Thank contributors publicly
- Ship v0.2 milestone as fast as possible — momentum compounds

---

## Quick Reference — Key URLs to Set Up

| Thing | URL |
|---|---|
| GitHub Org | github.com/ciel-dev |
| Main Repo | github.com/ciel-dev/ciel |
| Discord | discord.gg/ciel-dev |
| Twitter/X | twitter.com/ciel_dev |
| Docs (later) | ciel-dev.github.io |

---

You've got everything you need. Now go ship it. ☁️
