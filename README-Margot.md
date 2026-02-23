# Margot

Margot is a multi-agent AI desktop assistant with a Mem0-inspired four-layer memory system, 132+ tools spanning Google Cloud, GitHub, file operations, and specialized services, and a Coordinator agent (Grok 4.1 Fast, 2M token context) that orchestrates everything through a ReAct reasoning loop. Clients include a React/Tauri desktop app and a native iOS app.

## Repository Map

```
margot-backend/          FastAPI backend — agents, tools, memory, prompts, DB migrations
margot-ui/               React + TypeScript desktop UI (Tauri shell)
IOS-APP/                 Native iOS client (SwiftUI, iOS 17+)
agents.md                Full architecture reference (~4000 lines)
```

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Coordinator Agent                         │
│          (Grok 4.1 Fast via OpenRouter)                      │
│                                                              │
│  • ReAct loop orchestration (max 30 iterations)              │
│  • Tool routing and execution                                │
│  • Context assembly from memory layers                       │
│  • Enhanced reasoning (2M token context)                     │
└──────────────────┬──────────────────────────────────────────┘
                   │
       ┌───────────┼───────────────────────────────────┐
       │           │                                   │
┌──────▼─────┐ ┌───▼────────────┐ ┌────────────────────▼─────┐
│  Memory    │ │ Tool Services  │ │  Specialized Subagents   │
│  Manager   │ │                │ │                          │
│            │ │ • Web Search   │ │  • Nano Banana (Images)  │
│ • Short    │ │ • File Ops     │ │  • Veo 3.1 (Video)       │
│ • Semantic │ │ • GitHub       │ │  • Freya (Coding)        │
│ • Episodic │ │ • Google Cloud │ │  • Browser Agent (Web)   │
│ • Procedur │ │ • Voice TTS    │ │  • Light Research V2     │
└────────────┘ └────────────────┘ └──────────────────────────┘
```

## Core Agents

### Coordinator

**Model**: xAI Grok 4.1 Fast (`x-ai/grok-4.1-fast`) via OpenRouter

Primary orchestrator with a 2,000,000-token context window. Runs a ReAct loop (Reason → Act → Observe → Repeat) for up to 30 iterations, routing queries to 140+ tools with parallel execution where safe. Uses a 4-tier tool selection strategy: purpose-built tools (including Desktop Commander file tools) → shell fallback → browser navigation → direct answer. Shell safety guardrails block destructive operations and gate risky commands behind user confirmation. Independent tools run concurrently via `asyncio.gather()` for 30-50% faster multi-tool workflows. Can autonomously generate, validate, and hot-register new Python tools via the skill system. Manages background watches for proactive monitoring with pending alert injection.

### Freya — Coding Subagent

**Model**: MiniMax-M2 (`minimaxai/minimax-m2`) — 230B params, 10B active (MoE), 128K context

RAG-enhanced coding agent with a pgvector-backed best-practices corpus (18 categories). Features lint-on-save middleware (auto-linting with self-correction), git integration, sandboxed file access, and interleaved `<think>` reasoning. User-selected via toggle — bypasses the Coordinator entirely.

### Browser Agent

**Model**: Gemini 3 Flash (loop) / Grok 4.1 Fast (orchestration)

Intelligent browser automation using the **Ralph Loop** pattern — a stateless observe-verify-decide-execute loop with fresh DOM context each iteration. Controls Chrome via Playwright MCP. Multi-step workflows are decomposed by a `BrowserOrchestrator` into cycles of `RalphLoop` executions. Features include modal-aware DOM snapshots, element blacklisting, loop detection, fallback strategies, and a skills framework for recorded flows.

### Nano Banana Pro — Image Generation

**Model**: Gemini 3 Pro Image (`gemini-3-pro-image-preview`) via Google AI Studio

Text-to-image generation, image editing, and multi-image blending. Supports resolutions up to 4K, multiple aspect ratios, text rendering, Google Search grounding, and professional camera controls. One-shot chat integration — toggle enables, image generates, toggle resets.

### Veo 3.1 — Video Generation

**Model**: Veo 3.1 (`veo-3.1-generate-preview`) via Google AI Studio

Text-to-video, image-to-video, and frame interpolation. Generates 4-8 second clips at 720p or 1080p with native AI-generated audio. Supports video extension up to 148 seconds total.

### Light Research V2

Subtopic-based research pipeline: Planning → Parallel Research → Synthesis. Decomposes queries into 2-4 focused subtopics, researches them concurrently, and produces a formatted report with citations. ~$0.01 per query, 30-90 seconds.

### Deep Research V2

Premium 4-agent research system with cross-pollination. Generates 4 distinct research angles, assigns them to independent agents, extracts summaries, then runs a second round where agents review each other's findings. Produces a synthesis with consensus/disagreement analysis. ~$0.016 per query, 2-3 minutes.

## Memory System

Mem0-inspired four-layer architecture achieving 26% accuracy improvement over baseline. All layers use PostgreSQL with pgvector for vector similarity search.

| Layer | Purpose | Storage | Key Feature |
|-------|---------|---------|-------------|
| **Short-Term** | Current conversation context | PostgreSQL | Auto-updating metadata, pinning, pagination |
| **Semantic** | Long-term facts, preferences, entities | pgvector (HNSW) | Deduplication at >0.95 similarity, confidence scoring |
| **Episodic** | Conversation summaries | pgvector (HNSW) | Importance scoring with boost/decay, hybrid search |
| **Procedural** | Learned workflows and patterns | pgvector + GIN | Success-rate weighted scoring, versioning |

Context retrieval target: <150ms. Embedding model: OpenAI `text-embedding-3-small` (1536 dims).

## Tool Services

| Category | Count | Includes |
|----------|-------|----------|
| **Google Cloud** | 68 | GA4 analytics, Search Console, Merchant Center, Gmail, Drive, Sheets, Calendar, PageSpeed |
| **GitHub MCP** | 26 | Repos, branches, commits, issues, PRs, code search |
| **Desktop Commander** | 21 | File read/write/search, directory listing, terminal processes |
| **Web Search** | — | Firecrawl (scraping, search, content extraction) |
| **Knowledge Sources** | 5 | arXiv, PubMed, Wikipedia, Stack Exchange, NewsAPI |
| **ElevenLabs TTS** | — | Text-to-speech with multiple voices and streaming |
| **Nano Banana Pro** | 3 | Image generation, editing, blending |
| **Veo 3.1** | 3 | Text-to-video, image-to-video, frame interpolation |
| **Browser Control** | — | Playwright MCP, skills framework, Ralph Loop, 6 always-available lite tools |
| **Freya Coding** | 7 | read/write/search files, execute commands, git tools |
| **Shell Safety** | — | Pre-execution analysis, destructive op blocking, confirmation gates |
| **Skill System** | 3 | Generator, validator, executor with hot-registration (max 20 skills) |
| **Watch System** | 5 | create, list, toggle, delete, acknowledge watches + 7 evaluator types |

Google Cloud uses two OAuth connections: **Mountain Meadow** (productivity — Gmail, Drive, Sheets, Calendar) and **Gate Depot** (marketing — GA4, GSC, GMC).

## Research Tiers

| | Standard Chat | Light Research V2 | Deep Research V2 |
|---|---|---|---|
| **Architecture** | Single Grok call | Subtopic pipeline | 4 independent researchers |
| **Depth** | Single perspective | Subtopic decomposition | Angle-based with cross-pollination |
| **Criticism search** | Optional | Optional | Required |
| **Source credibility** | Basic | Basic | 4-tier assessment |
| **Report format** | Varies | Built-in `to_markdown()` | Structured with consensus |
| **Est. time** | 5-15s | 30-90s | 90-180s |
| **Est. cost** | ~$0.001 | ~$0.01 | ~$0.016 |

## Quick Start

### 1) Backend

```bash
cd margot-backend

python3.11 -m venv venv
source venv/bin/activate

pip install -r requirements.txt
cp .env.example .env

# Run migrations
alembic upgrade head

# Start API
uvicorn app.main:app --reload --host 127.0.0.1 --port 8000
```

### 2) Web/Desktop UI

```bash
cd margot-ui
npm install
npm run dev
```

### 3) Tauri Desktop App (optional)

```bash
cd margot-ui
npm run tauri dev
```

### 4) iOS App (optional)

Open `IOS-APP/Margot.xcodeproj` in Xcode and run the `Margot` scheme.

## Environment Variables

Start with `margot-backend/.env.example`, then set at least:

```bash
OPENROUTER_API_KEY=...

# PostgreSQL
POSTGRES_HOST=localhost
POSTGRES_PORT=5432
POSTGRES_DB=margot
POSTGRES_USER=margot
POSTGRES_PASSWORD=...
```

Common optional integrations:

- `OPENAI_API_KEY` — embeddings (text-embedding-3-small)
- `FIRECRAWL_API_KEY` — web search and scraping
- `GOOGLE_AI_STUDIO_API_KEY` — Nano Banana image gen, Veo 3.1 video gen
- `ELEVENLABS_API_KEY` — text-to-speech
- `GITHUB_TOKEN` — GitHub MCP tooling
- `REPLICATE_API_TOKEN` — legacy image generation

## Core API Routes

| Method | Route | Description |
|--------|-------|-------------|
| `GET` | `/api/health` | Health check |
| `POST` | `/api/chat/completions` | Main chat (Coordinator, research, browser, image, video modes) |
| `POST` | `/api/chat/coding` | Freya coding subagent |
| `POST` | `/api/images/generate` | Nano Banana image generation |
| `POST` | `/api/images/edit` | Nano Banana image editing |
| `POST` | `/api/images/blend` | Nano Banana image blending |
| `GET` | `/api/conversations` | Conversation list and history |
| `POST` | `/api/files/upload` | File uploads |
| `POST` | `/api/light-research/start` | Light Research V2 |
| `POST` | `/api/deep-research-v2/start` | Deep Research V2 |
| `POST` | `/api/tts/synthesize` | ElevenLabs text-to-speech |
| `GET` | `/api/browser/status` | Browser control connection status |
| `POST` | `/api/devices/register` | iOS device registration (APNs) |
| `GET` | `/api/skills` | Browser skill definitions |
| `GET` | `/api/generated-skills` | List generated skills (self-extending) |
| `DELETE` | `/api/generated-skills/{name}` | Delete a generated skill |

## Additional Documentation

- [`agents.md`](agents.md) — full architecture, services, and update history
- [`margot-backend/prompts/README.md`](margot-backend/prompts/README.md) — prompt loading behavior and validation
- [`margot-backend/MODEL_CONFIGURATION.md`](margot-backend/MODEL_CONFIGURATION.md) — model configuration notes

## License

Private project. All rights reserved.
