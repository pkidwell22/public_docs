**Patrick Kidwell**

patrick@mountainmeadowsystems.com | Website: [mountainmeadowsystems.com](https://mountainmeadowsystems.com/) | GitHub: pkidwell22

**Summary**

AI/ML Development Specialist with hands-on experience building autonomous LLM agents, multi-agent orchestration systems, and production-grade RAG pipelines. Built an AI desktop assistant featuring a Mem0-inspired four-layer memory architecture (26% accuracy improvement), 140+ integrated tools, seven specialized subagents (including Browser Agent for intelligent web automation), self-extending skill generation with AST-level validation, proactive monitoring via watch/heartbeat system, and multi-agent research systems with cross-pollination capabilities. Skilled in bridging core infrastructure with front-end applications across desktop (Tauri 2.0) and mobile (iOS/SwiftUI) platforms. Background in fine-tuning (QLoRA/LoRA), retrieval systems (pgvector/FAISS), and real-time streaming architectures.

**Core Skills**

* **Multi-Agent Systems:** Coordinator agents, ReAct loops (30 max iterations), subagent delegation, 4-tier tool selection strategy, parallel tool execution (30-50% faster), cross-pollination research, tool routing with \>95% accuracy, Ralph Loop pattern for browser automation, self-extending skill system with hot-registration  
* **Memory Architectures:** Mem0-inspired four-layer systems (short-term, semantic, episodic, procedural), pgvector embeddings, HNSW indexes, similarity threshold filtering, LLM-based memory extraction  
* **LLM Agent Engineering:** Grok 4.1 Fast (2M context), reasoning frameworks, tool-use systems, MCP servers, prompt caching, context management (80-95% token reduction), shell safety guardrails, proactive watch/alert systems  
* **RAG & Retrieval:** pgvector, FAISS, Chroma, document parsing, semantic search, context-aware retrieval, multi-tier source credibility assessment  
* **Fine-tuning & Training:** QLoRA, LoRA adapters, parameter-efficient methods (Mistral 7B, DeepSeek-Coder), adapter stacking  
* **Infrastructure & Deployment:** Tauri 2.0 (Rust), Docker, Cloud Run, FastAPI, PostgreSQL, Railway, Lambda Labs A100, Tailscale VPN  
* **APIs & Integrations:** OpenRouter, Google Cloud (GA4, GSC, GMC, Gmail, Drive, Calendar, Sheets), GitHub MCP, Desktop Commander, Twilio SMS, OAuth 2.0  
* **Frontend Development:** React, TypeScript, SwiftUI (iOS 17+), SSE streaming, real-time progress visualization, Vega-Lite chart rendering  
* **Backend Development:** Python, FastAPI, Node.js, Express, PostgreSQL, RESTful APIs, async processing, WebSockets

**Professional Projects**

**Margot: Multi-Agent AI Desktop & Mobile Assistant**

*Self-directed project | 2025 – Present*

* Architected production-grade AI assistant using Tauri 2.0 (Rust), FastAPI backend, React/TypeScript frontend, and PostgreSQL with pgvector for vector similarity search.  
* Built coordinator agent (Grok 4.1 Fast, 2M token context) orchestrating 140+ tools through ReAct loop pattern with up to 30 sequential operations per request and \>95% tool calling accuracy.  
* Implemented Mem0-inspired four-layer memory system achieving 26% accuracy improvement: short-term (conversation), semantic (facts with pgvector), episodic (summaries with importance decay), and procedural (learned workflows) with LLM-based extraction.  
* Developed seven specialized subagents: Freya (coding with MiniMax-M2, lint-on-save middleware, git integration), Nano Banana Pro (Gemini 3 Pro Image for generation/editing/blending), Veo 3.1 & Sora 2 (video generation with native audio), Light Research V2 (subtopic-based parallel research), Deep Research V2 (4-agent cross-pollination system), Browser Agent (Ralph Loop pattern for web automation), and Voice TTS.  
* Integrated 68 Google Cloud tools (GA4, GSC, GMC, Gmail, Drive, Sheets, Calendar with 13 tools), 26 GitHub MCP tools, 21 Desktop Commander tools, and Twilio SMS with multi-connection OAuth architecture featuring automatic tool-to-connection routing.  
* Built 4-tier tool selection strategy (LOBSTER sprint): purpose-built tools → shell fallback with safety guardrails → browser navigation via 6 always-available Browser Lite tools → direct answer. Shell safety system performs pre-execution analysis with three levels: auto-execute, user confirmation, and blocked destructive operations.  
* Implemented self-extending skill system: autonomous LLM-driven skill generation with AST-level validation (import whitelist, banned pattern detection, signature verification), pytest runner with 30s timeout, hot-registration into coordinator tool cache, and 20-skill cap with REST management API.  
* Built proactive watch/heartbeat system: 5 coordinator tools for watch lifecycle management, 7 evaluator types, flexible scheduling (slot/interval/cron), pending alert injection into coordinator context, and iOS notification-to-conversation flow with APNs integration.  
* Built Deep Research V2: six-phase multi-agent research system with 4 independent researchers (all Grok 4.1 Fast for reliable tool execution), dynamic angle generation, required criticism searches, 4-tier source credibility assessment, and cross-pollination between agents.  
* Built Browser Agent with Ralph Loop pattern: stateless iteration architecture with fresh context each loop, modal-aware DOM filtering, action verification, loop detection, and fallback strategies. LOBSTER2 enhancements added deterministic failure taxonomy with trace artifacts, bounded self-healing loop (trace → candidate build → validation → shadow replay), eval/gate/scorecard tooling for release readiness, and operations runbook.  
* Implemented Freya Harness upgrades: lint-on-save middleware (auto-linting Python/TypeScript/JS with self-correction), git integration (branch-based task isolation, auto-commit on lint pass, rollback via hard reset).  
* Implemented parallel tool calling (30-50% API call reduction), prompt caching (500ms+ faster), context management (80-95% token reduction), and worker compression (70-85% token savings).  
* Built Skills Framework V2 for browser automation: weighted scoring system (trigger/domain/URL/tag matching), token budget management (3000 tokens max), agent-aware filtering, re-evaluation on domain change or repeated failures, and 31 migrated browser skills with canonical directory structure.  
* Developed native iOS companion app with SwiftUI (iOS 17+), real-time SSE streaming via URLSessionDataDelegate, Vega-Lite chart rendering via WKWebView, advanced settings (agent toggles, model parameters), Tailscale VPN networking, glass UI design with conversation management, and APNs push notifications for watch alerts.

**Key Tech:** Python, TypeScript, Swift, FastAPI, React, SwiftUI, PostgreSQL, pgvector, Tauri 2.0, OpenRouter, Grok 4.1, MCP Servers, Docker, Tailscale, Playwright

**QLoRA Fine-Tuning & Model Adaptation**

*Self-directed project | 2025 – Present*

* Fine-tuned Mistral-7B with QLoRA adapters on curated 1B-token datasets (OpenOrca, Project Gutenberg, UltraChat).  
* Built adapter-stacking pipeline for general \+ stylistic generation tasks with scalable training on RTX 4060 and Lambda Labs A100 clusters.  
* Implemented dataset curation tools, evaluation metrics, WandB/TensorBoard logging, and interactive inference scripts.

**Key Tech:** Hugging Face Transformers, PEFT, bitsandbytes, accelerate, PyTorch CUDA, WandB

**Professional Experience**

**AI Solutions Developer – Prolutions**

*Freelance | 2025 – Present*

* Built an automated agentic system that runs every two weeks to analyze client websites via PageSpeed Insights and populate a shared Google Sheet with actionable performance recommendations for developer implementation.  
* Developed a conversational AI agent enabling non-technical staff to query Google Cloud Analytics APIs using natural language, translating requests into structured API calls and returning formatted insights.  
* Architected multi-agent orchestration systems modeled on production patterns established with Margot, applying coordinator-agent delegation, tool routing, and automated reporting pipelines across client engagements.  
* **Key Tech:** Python, Google Cloud APIs, PageSpeed Insights API, Google Sheets API, LLM Agents, Multi-Agent Orchestration

**Digital Marketing Specialist**

*Gate Depot, Millie’s Plants, Freelance | 2021 – Present*

* Managed $401k+ in Google Ads with ROAS \> 1,100%, $32,000 budget  
* Built custom GA4/GSC connectors to analyze traffic, revenue, and algorithmic impact.  
* Developed AI-powered reporting and automation scripts for campaign optimization.

**Digital Marketing Intern**

*Sharpenters (now Knife Aid) | May 2023 – January 2024*

* Analyzed historical Google Ads to find growth opportunities.  
* Launched seasonal Google Ads campaigns using keyword research and competitive analysis.  
* Optimized landing pages with UX best practices, improving user engagement.  
* Implemented conversion tracking via Google Analytics 4 and Google Tag Manager.  
* Conducted A/B testing on ad creatives and landing pages to refine campaign effectiveness.

**Education**

* **B.A. English Literature** – John Carroll University

**Certifications**

* Google Ads: Search, Shopping, Display, Measurement  
* Google Digital Marketing & E-commerce
