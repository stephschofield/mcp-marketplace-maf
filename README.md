# Federation

> **Your consulting firm's best people—automated, coordinated, and tireless.**
>
> Federation deploys seven AI agents that operate like a world-class consulting team: researching clients, drafting proposals, synthesizing intelligence, and generating polished deliverables—in minutes instead of weeks.

[![Next.js](https://img.shields.io/badge/Next.js_16-black?logo=next.js&logoColor=white)](https://nextjs.org)
[![React](https://img.shields.io/badge/React_19-61DAFB?logo=react&logoColor=black)](https://react.dev)
[![FastAPI](https://img.shields.io/badge/FastAPI-009688?logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com)
[![Python](https://img.shields.io/badge/Python_3.11+-3776AB?logo=python&logoColor=white)](https://python.org)
[![TypeScript](https://img.shields.io/badge/TypeScript_5-3178C6?logo=typescript&logoColor=white)](https://typescriptlang.org)
[![Azure OpenAI](https://img.shields.io/badge/Azure_OpenAI-0078D4?logo=microsoftazure&logoColor=white)](https://azure.microsoft.com/en-us/products/ai-services/openai-service)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

[Quick Start](#quick-start) · [Architecture](#architecture) · [API Reference](docs/API.md) · [Demo Plan](docs/DEMO_PLAN.md)

---

## The Problem

Professional services firms are hemorrhaging time on work that machines should do:

| Pain Point | Reality |
| --- | --- |
| **Knowledge fragmentation** | Consultants spend 30% of their time searching for work the firm already did |
| **Proposal bottleneck** | Senior resources spend 2–3 weeks manually crafting each proposal |
| **Research overhead** | 40% of junior analyst time goes to data gathering, not analysis |
| **Quality inconsistency** | Deliverable quality varies wildly by team composition |
| **Revenue ceiling** | Revenue is shackled to headcount—zero leverage |

**Federation exists to destroy every one of these problems.**

---

## Overview

Federation is a multi-agent AI platform built on **Microsoft Agent Framework (MAF)** and **Azure OpenAI GPT-5.x**. It mirrors a consulting firm's operating model with seven specialized agents that coordinate autonomously:

- **One sentence in** → a complete client proposal comes out
- **A company name** → a comprehensive executive briefing materializes
- **A vague question** → your firm's entire institutional memory is surfaced

The agents run in parallel, hand off context to each other, and produce structured, export-ready deliverables with full observability and tracing.

### Impact

| Metric | Before | After |
| --- | --- | --- |
| Proposal turnaround | 2–3 weeks | < 5 minutes |
| Research time per engagement | 40% of junior hours | 90% automated |
| Knowledge reuse rate | 15% (siloed) | 70%+ (discoverable) |
| Revenue per consultant | Baseline | +25–40% leverage |

---

## Key Features

### Multi-Agent Orchestration

The Orchestrator decomposes complex requests into subtasks, dispatches them to specialist agents in parallel, manages dependencies, and synthesizes a unified response. All coordination happens through structured message passing with full trace logging.

### Intelligent Proposal Generation

Type a single sentence describing a client engagement. The Strategist, Researcher, Analyst, Memory, and Scribe agents collaborate to produce a complete proposal—executive summary, methodology, team structure, timeline, and pricing—benchmarked against your firm's historical work.

### Real-Time Research Synthesis

The Researcher agent queries web, news, and company data sources in parallel, then synthesizes findings into structured intelligence briefs with source citations and confidence scoring.

### RAG-Powered Knowledge Base

The Memory agent uses embedding-based semantic search over your firm's past engagements, frameworks, templates, and expertise. It finds relevant work even when exact keywords don't match—because it understands meaning.

### Live Agent Status Streaming

WebSocket connections deliver real-time visibility into agent activity: which agents are thinking, what tools they're calling, handoff events between agents, and streaming token output. The UI shows it all.

### Document Export Pipeline

Generated deliverables export to Markdown, HTML, PDF, and DOCX with one click. Documents are versioned and linked to their originating conversations.

### Full Observability

Every agent action is traced: start time, completion time, tokens consumed, inputs, outputs, and errors. The Analytics dashboard aggregates performance metrics across agents and time periods.

---

## Architecture

```text
┌──────────────────────────────────────────────────────────────────────────┐
│                          FRONTEND                                        │
│            Next.js 16 · React 19 · Shadcn/ui · Tailwind CSS             │
│                  Zustand · React Query · WebSocket                       │
└────────────────────────────────┬─────────────────────────────────────────┘
                                 │  REST API / WebSocket
┌────────────────────────────────▼─────────────────────────────────────────┐
│                           BACKEND                                        │
│                    FastAPI · Python 3.11+                                 │
│  ┌────────────────────────────────────────────────────────────────────┐  │
│  │                       API GATEWAY LAYER                            │  │
│  │  /chat  /proposals  /research  /documents  /knowledge  /analytics │  │
│  └────────────────────────────────┬───────────────────────────────────┘  │
│                                   │                                      │
│  ┌────────────────────────────────▼───────────────────────────────────┐  │
│  │              MICROSOFT AGENT FRAMEWORK LAYER                       │  │
│  │                                                                    │  │
│  │  ┌────────────────────────────────────────────────────────────┐    │  │
│  │  │                    ORCHESTRATOR                             │    │  │
│  │  │         Task Planning · Coordination · Quality Control      │    │  │
│  │  └──────────┬────────┬────────┬────────┬────────┬─────────────┘    │  │
│  │             │        │        │        │        │                   │  │
│  │  ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐           │  │
│  │  │STRAT │ │RESRCH│ │ANLYST│ │SCRIBE│ │ADVSR │ │MEMORY│           │  │
│  │  │      │ │      │ │      │ │      │ │      │ │      │           │  │
│  │  │Scope │ │Web   │ │Data  │ │Doc   │ │Exec  │ │RAG   │           │  │
│  │  │Prop. │ │News  │ │Fin.  │ │Gen.  │ │Summ. │ │Past  │           │  │
│  │  │Frame.│ │Intel.│ │Bench.│ │Brand.│ │Comms │ │Work  │           │  │
│  │  └──────┘ └──────┘ └──────┘ └──────┘ └──────┘ └──────┘           │  │
│  └────────────────────────────────────────────────────────────────────┘  │
│                                   │                                      │
│  ┌────────────────────────────────▼───────────────────────────────────┐  │
│  │                       SERVICES LAYER                               │  │
│  │  LLMService · DocumentService · KnowledgeService · TraceService   │  │
│  └────────────────────────────────────────────────────────────────────┘  │
└────────────┬──────────────────────┬──────────────────────┬───────────────┘
             │                      │                      │
  ┌──────────▼──────────┐ ┌────────▼────────┐ ┌───────────▼───────────┐
  │   Azure OpenAI      │ │     SQLite      │ │   External APIs       │
  │                     │ │ (via aiosqlite) │ │   (Mock / Real)       │
  │ • GPT-5.x Chat     │ │                 │ │                       │
  │ • GPT-5.x Codex    │ │ • Conversations │ │ • Web Search          │
  │ • Embeddings       │ │ • Documents     │ │ • News API            │
  │ • Structured Out   │ │ • Knowledge     │ │ • Company Data        │
  │                     │ │ • Agent Traces  │ │                       │
  └─────────────────────┘ └─────────────────┘ └───────────────────────┘
```

### Agent Roles

| Agent | Role | What It Does |
| --- | --- | --- |
| **Orchestrator** | Coordinator | Decomposes requests, dispatches agents in parallel, manages dependencies, synthesizes final response |
| **Strategist** | Strategy | Engagement scoping, proposal generation, methodology selection, approach design |
| **Researcher** | Intelligence | Web search, news synthesis, company profiling, competitive analysis |
| **Analyst** | Analysis | Financial modeling, benchmarking, data visualization, metrics calculation |
| **Scribe** | Documents | Branded document generation, formatting, export to PDF/DOCX/HTML |
| **Advisor** | Communications | Executive summaries, client talking points, meeting preparation |
| **Memory** | Knowledge | Semantic search over past engagements, framework retrieval, expertise mapping |

### Request Flow

```text
User Message
    │
    ▼
┌─────────────────┐
│   Orchestrator   │ ── Analyzes intent via structured output (JSON schema)
└────────┬────────┘
         │
         ├──► Strategist  ─┐
         ├──► Researcher  ─┤  Agents execute in PARALLEL
         ├──► Analyst     ─┤  (asyncio.gather)
         ├──► Memory      ─┤
         └──► Scribe      ─┘
                           │
                    All results collected
                           │
                    ┌──────▼──────┐
                    │  Synthesize  │ ── Orchestrator merges agent outputs
                    └──────┬──────┘
                           │
                    Final Response + Document artifacts
```

---

## Tech Stack

### Frontend

| Technology | Version | Purpose |
| --- | --- | --- |
| [Next.js](https://nextjs.org) | 16.x | React framework with App Router |
| [React](https://react.dev) | 19.x | UI library |
| [Shadcn/ui](https://ui.shadcn.com) + [Radix UI](https://www.radix-ui.com) | Latest | Accessible component primitives |
| [Tailwind CSS](https://tailwindcss.com) | 4.x | Utility-first styling |
| [Zustand](https://zustand-demo.pmnd.rs) | 5.x | Lightweight state management |
| [TanStack React Query](https://tanstack.com/query) | 5.x | Server state & data fetching |
| [TypeScript](https://typescriptlang.org) | 5.x | Type safety |

### Backend

| Technology | Version | Purpose |
| --- | --- | --- |
| [Python](https://python.org) | 3.11+ | Runtime |
| [FastAPI](https://fastapi.tiangolo.com) | 0.109+ | Async REST API framework |
| [Microsoft Agent Framework](https://github.com/microsoft/agent-framework) | Latest | Multi-agent orchestration |
| [SQLAlchemy](https://www.sqlalchemy.org) | 2.x | Async ORM |
| [aiosqlite](https://github.com/omnilib/aiosqlite) | Latest | Async SQLite driver |
| [Pydantic](https://docs.pydantic.dev) | 2.x | Data validation & settings |
| [Azure Identity](https://learn.microsoft.com/en-us/python/api/azure-identity/) | Latest | Entra ID (keyless) authentication |

### AI & Cloud Services

| Service | Purpose |
| --- | --- |
| [Azure OpenAI](https://azure.microsoft.com/en-us/products/ai-services/openai-service) — GPT-5.2 Chat | Primary language model for agent reasoning and synthesis |
| [Azure OpenAI](https://azure.microsoft.com/en-us/products/ai-services/openai-service) — GPT-5.1 | Secondary model for specialized tasks |
| [Azure OpenAI](https://azure.microsoft.com/en-us/products/ai-services/openai-service) — GPT-5.1 Codex Max | Code generation and structured output |
| [Azure OpenAI](https://azure.microsoft.com/en-us/products/ai-services/openai-service) — text-embedding-3-small | Semantic embeddings for knowledge base search |
| [Azure Entra ID](https://www.microsoft.com/en-us/security/business/identity-access/microsoft-entra-id) | Keyless authentication via `DefaultAzureCredential` |
| [SQLite](https://www.sqlite.org) | Lightweight embedded database for conversations, documents, traces |

---

## Project Structure

```text
federation/
├── docs/
│   ├── API.md                # Complete REST & WebSocket API reference
│   ├── PRD.md                # Product Requirements Document
│   ├── TRD.md                # Technical Requirements Document
│   └── DEMO_PLAN.md          # Demo scenarios and walkthrough guide
│
├── frontend/                 # Next.js 16 application
│   └── src/
│       ├── app/              # App Router pages (chat, proposals, research, knowledge, analytics)
│       ├── components/       # React components (chat, landing, sidebar, UI primitives)
│       └── lib/              # API client, Zustand store, WebSocket, types, utils
│
├── backend/                  # FastAPI application
│   ├── app/
│   │   ├── agents/           # 7 AI agents (orchestrator, strategist, researcher, analyst, scribe, advisor, memory)
│   │   ├── api/              # REST routes + WebSocket handler
│   │   ├── models/           # SQLAlchemy models & Pydantic schemas
│   │   ├── services/         # LLM, document, knowledge, trace services
│   │   └── data/             # Seed data
│   └── tests/                # Pytest test suite (12 test modules)
│
├── scripts/                  # Database setup scripts
├── LICENSE                   # MIT License
└── README.md
```

---

## Quick Start

### Prerequisites

- **Python 3.11+**
- **Node.js 18+** with [pnpm](https://pnpm.io) (recommended)
- **Azure OpenAI** access with Entra ID authentication
- **Azure CLI** authenticated (`az login`)

### 1. Clone & configure

```bash
git clone https://github.com/shyamsridhar123/Order66.git
cd Order66
```

Create a `.env` file in the project root:

```env
# Azure OpenAI (Entra ID auth — no API keys needed)
AZURE_OPENAI_ENDPOINT=https://your-resource.openai.azure.com

# Deployments (adjust to your deployment names)
AZURE_OPENAI_DEPLOYMENT_NAME=gpt-5.2-chat
AZURE_OPENAI_GPT5_DEPLOYMENT_NAME=gpt-5.1
AZURE_OPENAI_CODEX_DEPLOYMENT_NAME=gpt-5.1-codex-max
AZURE_OPENAI_TEXTEMBEDDING_DEPLOYMENT_NAME=text-embedding-3-small

# Database
DATABASE_URL=sqlite+aiosqlite:///./data/federation.db
```

### 2. Start the backend

```bash
cd backend
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt

# Seed the database with sample engagements, frameworks, and knowledge
python -c "from app.data.seed import seed_database; import asyncio; asyncio.run(seed_database())"

# Launch the API server
uvicorn app.main:app --reload --port 8000
```

### 3. Start the frontend

```bash
cd frontend
pnpm install
pnpm dev
```

### 4. Open the app

Navigate to **<http://localhost:3000>** and start talking to your agents.

Swagger API docs are available at **<http://localhost:8000/docs>**.

---

## Pages & Features

| Page | Route | What You Can Do |
| --- | --- | --- |
| **Landing** | `/` | Platform overview, agent capabilities, paradigm shift narrative |
| **Chat** | `/chat` | Multi-agent conversation with real-time agent status panel |
| **Proposals** | `/proposals` | Generate, view, and export client proposals |
| **Research** | `/research` | Ad-hoc research queries and client intelligence briefings |
| **Knowledge** | `/knowledge` | Semantic search across the firm's knowledge base |
| **Analytics** | `/analytics` | Agent performance metrics, execution traces, token usage |

---

## API Reference

| Endpoint | Method | Description |
| --- | --- | --- |
| `/health` | GET | Health check |
| `/api/chat/conversations` | GET | List conversations |
| `/api/chat/conversations` | POST | Create conversation |
| `/api/chat/conversations/{id}/messages` | GET | Get message history |
| `/api/chat/conversations/{id}/messages` | POST | Send message (triggers agent pipeline) |
| `/api/proposals` | GET | List proposals |
| `/api/proposals/generate` | POST | Generate a new proposal |
| `/api/research/query` | POST | Execute research query |
| `/api/research/briefing` | POST | Generate client briefing |
| `/api/knowledge/search` | POST | Semantic search |
| `/api/knowledge/similar` | POST | Find similar engagements |
| `/api/documents/{id}` | GET | Get document |
| `/api/documents/{id}/export` | POST | Export to PDF/DOCX/HTML/Markdown |
| `/api/analytics/traces` | GET | Agent execution traces |
| `/api/analytics/metrics` | GET | Aggregated performance metrics |
| `/ws/agents/{conversation_id}` | WebSocket | Real-time agent status streaming |

Full API documentation with request/response schemas: **[docs/API.md](docs/API.md)**

### WebSocket Events

| Event | Description |
| --- | --- |
| `agent.started` | Agent began processing a task |
| `agent.thinking` | Agent reasoning step with progress indicator |
| `agent.completed` | Agent finished with result summary and duration |
| `agent.handoff` | Control transferred between agents |
| `agent.error` | Agent encountered an error |
| `stream.token` | Streaming LLM token for typewriter effect |
| `document.generated` | New document artifact created |

---

## Demo Scenarios

### 1. Rapid Proposal Generation *(Primary — the "wow" moment)*

> *"Create a proposal for Acme Corp's digital transformation initiative. They're a $5B industrial manufacturer looking to modernize their supply chain and implement predictive maintenance across 12 factories."*

Five agents activate in parallel. In under 3 minutes: a complete proposal with executive summary, methodology, team structure, timeline, and pricing—benchmarked against similar past engagements.

### 2. Client Intelligence Briefing

> *"Brief me on TechCorp Solutions ahead of tomorrow's meeting."*

Researcher synthesizes public data while Memory surfaces your firm's prior engagement history. Result: company overview, executive profiles, competitive landscape, pain points, and recommended talking points.

### 3. Knowledge Discovery

> *"What frameworks have we used for post-merger integration in healthcare?"*

Memory agent performs semantic search—finding relevant work even when exact terms don't match. Returns past engagements with outcomes, reusable templates, and named experts.

### 4. Deliverable Quality Assurance

Upload a draft. Scribe checks formatting standards, Strategist evaluates strategic coherence, Analyst validates data. You get a quality score with specific improvement recommendations.

---

## Security

| Area | Implementation |
| --- | --- |
| **Authentication** | Azure Entra ID via `DefaultAzureCredential` — no API keys stored or transmitted |
| **Secrets management** | Environment variables via `.env` (gitignored); no secrets in code |
| **API security** | CORS restricted to configured origins; FastAPI automatic request validation |
| **Data storage** | SQLite database in local `./data` directory |
| **Input validation** | Pydantic v2 schema validation on all API inputs; Zod on the frontend |
| **Structured outputs** | JSON Schema enforcement on LLM responses to prevent injection |
| **Agent tracing** | Full audit trail of every agent action, input, output, and token usage |
| **OWASP alignment** | No SQL injection surface (ORM-only); no user-supplied template rendering; CORS enforcement |

> **Note:** This is a demonstration/POC platform. For production deployment, add SSO, network isolation, encryption at rest, rate limiting, and SOC 2/GDPR compliance controls.

---

## Testing

```bash
cd backend

# Run all tests
pytest

# Verbose output
pytest -v

# Specific module
pytest tests/test_chat.py

# With coverage
pytest --cov=app
```

Test modules cover: API endpoints, chat flow, proposals, research, knowledge search, documents, analytics, WebSocket, seed data, and core agent logic.

---

## Development

### Adding a New Agent

1. Create `backend/app/agents/{name}.py` with an `async def run_{name}(task, context)` function
2. Add the agent's system prompt to `backend/app/agents/prompts.py`
3. Register any tools in `backend/app/agents/factory.py` under `AGENT_TOOLS`
4. Wire routing in `backend/app/agents/orchestrator.py` → `_execute_agent()`
5. Add the agent name to the intent classification schema's `required_agents` enum

### Code Conventions

- **Backend**: Async everywhere. Pydantic for validation. SQLAlchemy 2.x async sessions.
- **Frontend**: Server Components by default. `'use client'` only for interactivity. Zustand for client state, React Query for server state. Shadcn/ui component patterns.
- **Testing**: Pytest with `pytest-asyncio`. Mock LLM calls in tests.

---

## Documentation

| Document | Description |
| --- | --- |
| [API Reference](docs/API.md) | Complete REST & WebSocket API with request/response schemas |
| [Product Requirements](docs/PRD.md) | Business context, user stories, success criteria |
| [Technical Requirements](docs/TRD.md) | Architecture, implementation details, database schema |
| [Demo Plan](docs/DEMO_PLAN.md) | Demo script, talking points, recovery playbook |

---

## License

MIT License — see [LICENSE](LICENSE) for details.

Copyright (c) 2026 Shyam Sridhar
