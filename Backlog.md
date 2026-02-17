# Backlog

> *"I don't have time to explain things twice. Read this."*

Last updated: 2026-02-17

---

## Completed

| Task | Notes |
|------|-------|
| Initial setup | Beth agent system installed |
| Switch to Entra ID auth | Removed API key auth from config, LLM service, and MAF factory. Now uses `DefaultAzureCredential` everywhere. No secrets in .env. |
| Fix pip dependency hell | Replaced `agent-framework` metapackage (pulled 15+ sub-packages) with `agent-framework-core` + `agent-framework-azure`. Install went from infinite backtracking to ~30 seconds. |
| Fix MAF import | `ai_function` → `tool` (correct export name in current MAF version) |
| App startup | Backend (FastAPI :8000) and frontend (Next.js :3001) both running with Entra ID auth |
| Comprehensive README rewrite | Full README with hook, overview, key features, architecture diagrams, tech stack tables, security section, services, demo scenarios, MIT license. Lint-clean. |
| Frontend rebuild agent prompt | Created `docs/FRONTEND_REBUILD_PROMPT.md` — 500+ line comprehensive prompt spec for an agent to rebuild the entire frontend from scratch. Covers tech stack, design system (oklch tokens, agent colors, animations), all 6 pages with pixel-level detail, Zustand store, API client, WebSocket client, TypeScript types, Playwright E2E tests. Based on full analysis of README, PRD, TRD, API docs, and reading every existing frontend file. |

---

## In Progress

*Nothing currently in progress.*

### Follow-ups

- [ ] **Seed database** — Run seed script to populate demo data (mcp-marketplace-maf-pia)
- [ ] **Verify deployment names** — Confirm gpt-5.2-chat etc. exist in Azure resource (mcp-marketplace-maf-2n8)
- [ ] **Update docs for Entra ID** — README.md and TRD.md still reference API keys (mcp-marketplace-maf-033)

---

## Backlog (Prioritized)

### High Priority (P1)

- [ ] **Your first task** — Describe what needs to be done

### Medium Priority (P2)

- [ ] **Future work** — Things to do later

### Low Priority (P3)

- [ ] **Nice to have** — When you have time

---

## Decisions

| Decision | Rationale | Date |
|----------|-----------|------|
| Use Beth orchestrator | Coordinated multi-agent workflows | Today |
| Entra ID over API keys | No secrets to leak, uses `az login` identity, aligns with zero-trust | 2026-02-17 |
| Slim MAF install | `agent-framework-core` + `agent-framework-azure` only — no Anthropic, Ollama, Redis, etc. | 2026-02-17 |

---

## Status Summary

**For Leadership:**

Project initialized with Beth agent system.

**What's Working:**

- Beth agent (orchestrator) — Ready
- Full agent roster — Ready
- All skills — Loaded

**What's Coming:**

- Your roadmap here

**Blockers:** None.

---

## How We Track Work

This file is the single source of truth. When you start work:

1. Move the task to **In Progress**
2. Do the work
3. Move to **Completed** when done
4. Commit changes

No external tools. No databases. Just this markdown file.

---

*"Now you know what's happening. Questions? I'll answer them. Complaints? Keep them to yourself."*
