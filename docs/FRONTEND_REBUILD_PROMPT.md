# Federation Frontend Rebuild — Agent Prompt

> **Mission**: Rebuild the Federation frontend from scratch. The backend API already exists and is fully functional at `http://localhost:8000`. You are building ONLY the frontend — a Next.js 16 application that communicates with this backend via REST and WebSocket APIs.

---

## 1. Project Identity

**Federation** is a multi-agent AI platform for professional services firms. It deploys 7 specialized AI agents (Orchestrator, Strategist, Researcher, Analyst, Scribe, Advisor, Memory) that coordinate autonomously to produce consulting deliverables — proposals, research briefings, knowledge discovery, and analytics dashboards.

The frontend is the user-facing interface that lets consultants interact with these agents via a chat-first experience, with dedicated pages for proposals, research, knowledge search, and analytics.

---

## 2. Tech Stack (Exact Versions)

| Technology | Version | Purpose |
|---|---|---|
| Next.js | 16.x | React framework, App Router (no Pages Router) |
| React | 19.x | UI library |
| TypeScript | 5.x | Strict mode, no `any` |
| Tailwind CSS | 4.x | Utility-first styling via `@tailwindcss/postcss` |
| shadcn/ui | new-york style | Accessible component primitives (Radix UI based) |
| Zustand | 5.x | Client state management |
| TanStack React Query | 5.x | Server state, data fetching, cache |
| lucide-react | 0.563+ | Icons |
| next-themes | 0.4.x | Dark/light mode toggle |
| uuid | 13.x | Client-side ID generation |
| class-variance-authority | 0.7.x | Component variant styling |
| clsx + tailwind-merge | Latest | Conditional classnames via `cn()` utility |
| tw-animate-css | 1.4+ | Animation utilities |
| pnpm | Latest | Package manager |
| Playwright | Latest | E2E testing |

### Key Configuration

**PostCSS** (`postcss.config.mjs`):
```js
const config = { plugins: { "@tailwindcss/postcss": {} } };
export default config;
```

**shadcn/ui** (`components.json`):
```json
{
  "$schema": "https://ui.shadcn.com/schema.json",
  "style": "new-york",
  "rsc": true,
  "tsx": true,
  "tailwind": {
    "config": "",
    "css": "src/app/globals.css",
    "baseColor": "neutral",
    "cssVariables": true,
    "prefix": ""
  },
  "iconLibrary": "lucide",
  "aliases": {
    "components": "@/components",
    "utils": "@/lib/utils",
    "ui": "@/components/ui",
    "lib": "@/lib",
    "hooks": "@/hooks"
  }
}
```

**TypeScript paths**: `@/*` maps to `./src/*`.

**Fonts**: Geist Sans and Geist Mono via `next/font/google`, exposed as CSS variables `--font-geist-sans` and `--font-geist-mono`.

---

## 3. Project Structure

Build exactly this file tree:

```
frontend/
├── package.json
├── tsconfig.json
├── next.config.ts
├── postcss.config.mjs
├── components.json
├── eslint.config.mjs
├── public/
└── src/
    ├── app/
    │   ├── globals.css              # Theme tokens, custom animations
    │   ├── layout.tsx               # Root layout with Providers wrapper
    │   ├── page.tsx                 # Landing page (marketing/overview)
    │   ├── chat/
    │   │   ├── layout.tsx           # Sidebar + AgentStatusPanel + main content
    │   │   └── page.tsx             # Chat interface
    │   ├── proposals/
    │   │   ├── layout.tsx           # Sidebar + AgentStatusPanel + main content
    │   │   └── page.tsx             # Proposal generation & list
    │   ├── research/
    │   │   ├── layout.tsx           # Sidebar + AgentStatusPanel + main content
    │   │   └── page.tsx             # Research query + client briefing tabs
    │   ├── knowledge/
    │   │   ├── layout.tsx           # Sidebar + AgentStatusPanel + main content
    │   │   └── page.tsx             # Knowledge base search & browse
    │   └── analytics/
    │       ├── layout.tsx           # Sidebar + AgentStatusPanel + main content
    │       └── page.tsx             # Metrics cards, agent performance, trace history
    ├── components/
    │   ├── providers.tsx            # QueryClientProvider + ThemeProvider
    │   ├── sidebar.tsx              # Fixed left navigation
    │   ├── agent-status-panel.tsx   # Fixed right panel showing agent states
    │   ├── theme-toggle.tsx         # Dark/light mode button
    │   ├── chat/
    │   │   ├── chat-interface.tsx   # Full chat UI with conversation sidebar
    │   │   ├── conversation-list.tsx # Conversation list sidebar
    │   │   └── index.ts            # Barrel export
    │   ├── landing/
    │   │   ├── hero-section.tsx
    │   │   ├── paradigm-shift-section.tsx
    │   │   ├── agents-section.tsx
    │   │   ├── capabilities-section.tsx
    │   │   ├── roadmap-section.tsx
    │   │   ├── footer-section.tsx
    │   │   ├── landing-nav.tsx
    │   │   └── index.ts            # Barrel export
    │   └── ui/                     # shadcn/ui primitives (install via CLI)
    │       ├── avatar.tsx
    │       ├── badge.tsx
    │       ├── button.tsx
    │       ├── card.tsx
    │       ├── dialog.tsx
    │       ├── dropdown-menu.tsx
    │       ├── input.tsx
    │       ├── scroll-area.tsx
    │       ├── separator.tsx
    │       ├── sheet.tsx
    │       ├── skeleton.tsx
    │       ├── tabs.tsx
    │       ├── textarea.tsx
    │       └── tooltip.tsx
    └── lib/
        ├── api.ts                  # REST API client (typed, namespace pattern)
        ├── store.ts                # Zustand store (conversations, messages, agents, UI)
        ├── types.ts                # TypeScript types matching backend schemas
        ├── utils.ts                # cn() utility
        └── websocket.ts            # WebSocket client + React hook
```

---

## 4. Design System & Theming

### 4.1 Color System

Use oklch-based CSS variables with full light and dark themes. The theme uses a **neutral base** with a monochromatic primary palette.

**Light theme** (`:root`):
```css
--background: oklch(1 0 0);
--foreground: oklch(0.145 0 0);
--card: oklch(1 0 0);
--card-foreground: oklch(0.145 0 0);
--primary: oklch(0.205 0 0);
--primary-foreground: oklch(0.985 0 0);
--secondary: oklch(0.97 0 0);
--muted: oklch(0.97 0 0);
--muted-foreground: oklch(0.556 0 0);
--accent: oklch(0.97 0 0);
--destructive: oklch(0.577 0.245 27.325);
--border: oklch(0.922 0 0);
--input: oklch(0.922 0 0);
--ring: oklch(0.708 0 0);
--radius: 0.625rem;
```

**Dark theme** (`.dark`):
```css
--background: oklch(0.145 0 0);
--foreground: oklch(0.985 0 0);
--card: oklch(0.205 0 0);
--card-foreground: oklch(0.985 0 0);
--primary: oklch(0.922 0 0);
--primary-foreground: oklch(0.205 0 0);
--secondary: oklch(0.269 0 0);
--muted: oklch(0.269 0 0);
--muted-foreground: oklch(0.708 0 0);
--border: oklch(1 0 0 / 10%);
--input: oklch(1 0 0 / 15%);
```

**Chart colors**: 5 chart variables for analytics visualizations (chart-1 through chart-5) — warm accents in light mode, vibrant hues in dark mode.

**Sidebar colors**: Dedicated sidebar CSS variables for background, foreground, primary, accent, border, and ring — enabling the sidebar to have its own color scheme.

### 4.2 Agent Color Map

Each of the 7 agents has a consistent color identity used across the agent status panel, analytics, and landing page:

| Agent | Status Panel Color | Landing Gradient |
|---|---|---|
| Orchestrator | `text-purple-500` / `bg-purple-500` | `from-violet-500 to-purple-600` |
| Strategist | `text-blue-500` / `bg-blue-500` | `from-blue-500 to-cyan-500` |
| Researcher | `text-green-500` / `bg-green-500` | `from-emerald-500 to-green-500` |
| Analyst | `text-orange-500` / `bg-orange-500` | `from-orange-500 to-amber-500` |
| Scribe | `text-pink-500` / `bg-pink-500` | `from-pink-500 to-rose-500` |
| Advisor | `text-cyan-500` / `bg-cyan-500` | `from-indigo-500 to-blue-600` |
| Memory | `text-yellow-500` / `bg-yellow-500` | `from-teal-500 to-cyan-600` |

### 4.3 Agent Icons

| Agent | Icon (lucide-react) |
|---|---|
| Orchestrator | `GitBranch` |
| Strategist | `Brain` (landing: `Target`) |
| Researcher | `Search` |
| Analyst | `TrendingUp` (landing: `BarChart3`) |
| Scribe | `FileEdit` |
| Advisor | `Users` (landing: `MessageSquare`) |
| Memory | `Database` |

### 4.4 Custom Animations

Define these keyframes and utility classes in `globals.css`:

| Class | Effect | Duration |
|---|---|---|
| `.animate-fade-in-up` | Fade in + translate up 20px | 0.6s ease-out |
| `.animate-fade-in` | Simple opacity fade | 0.4s ease-out |
| `.animate-slide-in-right` | Fade in + translate right 20px | 0.5s ease-out |
| `.animate-pulse-glow` | Box-shadow pulse on primary color | 2s infinite |
| `.stagger-children > *:nth-child(n)` | Stagger fade-in-up for first 7 children | 0.1s–0.7s delays |

### 4.5 Typography

- **Body**: Geist Sans (`--font-geist-sans`)
- **Monospace**: Geist Mono (`--font-geist-mono`)
- **Antialiased**: `antialiased` class on body
- **Smooth scrolling**: `scroll-behavior: smooth` on html

---

## 5. Application Shell & Layout

### 5.1 Root Layout (`src/app/layout.tsx`)

Server Component. Wraps everything in:
1. `<html lang="en" suppressHydrationWarning>` (required for next-themes)
2. `<body>` with Geist font CSS variables and `antialiased`
3. `<Providers>` wrapper (client component)

Metadata: `title: "Federation - Multi-Agent AI Platform"`, `description: "Coordinated AI agents for professional services"`.

### 5.2 Providers (`src/components/providers.tsx`)

Client Component. Wraps children with:
1. **QueryClientProvider** — TanStack React Query with `staleTime: 60000`, `refetchOnWindowFocus: false`
2. **ThemeProvider** (next-themes) — `attribute="class"`, `defaultTheme="system"`, `enableSystem`, `disableTransitionOnChange`

### 5.3 App Page Layouts

All inner pages (`/chat`, `/proposals`, `/research`, `/knowledge`, `/analytics`) share the SAME layout pattern:

```tsx
<div className="flex h-screen">
  <Sidebar />                           {/* Fixed left, 256px wide */}
  <main className="flex-1 lg:ml-64 xl:mr-72 flex">
    <div className="flex-1 flex flex-col">
      {children}                        {/* Page content */}
    </div>
    <AgentStatusPanel />                {/* Fixed right, 288px wide, xl+ only */}
  </main>
</div>
```

The landing page (`/`) does NOT use this layout — it has its own full-width design with `LandingNav`.

### 5.4 Sidebar (`src/components/sidebar.tsx`)

Client Component. Fixed left sidebar, 256px (`w-64`).

**Structure**:
- **Logo bar** (h-16): "F" logo in primary-colored rounded box + "Federation" text, links to `/`
- **Navigation links**: 5 items, each with icon + label
  - Chat → `/chat` → `MessageSquare`
  - Proposals → `/proposals` → `FileText`
  - Research → `/research` → `Search`
  - Knowledge → `/knowledge` → `BookOpen`
  - Analytics → `/analytics` → `BarChart3`
- **Active state**: `bg-primary/10 text-primary`
- **Inactive state**: `text-muted-foreground hover:bg-muted hover:text-foreground`
- **Footer**: Theme toggle + "Multi-Agent AI Platform / for Professional Services" text
- **Mobile**: Toggle button (fixed top-left, z-50, `lg:hidden`), sidebar slides from left with `-translate-x-full` / `translate-x-0` transition

### 5.5 Agent Status Panel (`src/components/agent-status-panel.tsx`)

Client Component. Fixed right panel, 288px (`w-72`). Only visible on `xl:` breakpoint and above.

**Structure**:
- **Header** (h-16): "Agent Activity" title + badge showing count of active agents
- **Scrollable list** of 7 agent cards, each showing:
  - Agent icon (color-coded) + agent label
  - Status badge: `idle` | `thinking` | `executing` | `waiting` | `completed` | `error`
  - Current task text (when not idle, shown as 2-line clamp)
  - **Thinking indicator**: 3 pulsing dots (yellow)
  - **Executing indicator**: Animated progress bar (blue)
  - **Active highlight**: `border-primary/50 bg-primary/5` when not idle

**Status badge colors**:
| Status | Light | Dark |
|---|---|---|
| idle | `bg-muted text-muted-foreground` | same |
| thinking | `bg-yellow-100 text-yellow-800` | `bg-yellow-900 text-yellow-200` |
| executing | `bg-blue-100 text-blue-800` | `bg-blue-900 text-blue-200` |
| waiting | `bg-purple-100 text-purple-800` | `bg-purple-900 text-purple-200` |
| completed | `bg-green-100 text-green-800` | `bg-green-900 text-green-200` |
| error | `bg-red-100 text-red-800` | `bg-red-900 text-red-200` |

---

## 6. Pages — Detailed Specifications

### 6.1 Landing Page (`/`)

Full-width marketing page. No sidebar. Composed of 7 sections:

#### LandingNav
- Fixed header, transparent initially, then `bg-background/80 backdrop-blur-lg` on scroll
- Left: Logo + "Federation" text
- Center (desktop): 4 hash links — Platform, Agents, Capabilities, Implementation
- Right: ThemeToggle + "API Docs" link (to `http://localhost:8000/docs`) + "Launch App" button (to `/chat`)
- Mobile: Hamburger menu that expands to show links

#### HeroSection
- Full viewport height (`min-h-screen`), centered content
- Background: Gradient overlay + subtle grid pattern + two floating blur orbs (animate-pulse)
- Badge: "Powered by Microsoft Agent Framework" with Sparkles icon
- Headline: `text-5xl md:text-7xl`, "Your firm's **collective intelligence**, amplified." with gradient text on "collective intelligence"
- Subtitle: Text about ambient reasoning layer, "Goal and delegate—not find and navigate."
- **Intent Delegate input**: Styled input box with placeholder cycling through 4 sample prompts, Send button links to `/chat`
- Sample prompt buttons: 3 clickable pills that populate the input
- CTA buttons: "Start Collaborating" (primary) + "Meet the Agents" (outline, scrolls to #agents)
- Stats row: 4 cards — "7 Specialized Agents", "< 5 min Proposal Generation", "Instant Client Briefings", "24/7 Availability"
- Scroll indicator: Bouncing pill at bottom

#### ParadigmShiftSection (`#platform`)
- Before/After toggle (`bg-muted/30` background)
- 4 comparison cards showing old way vs. Federation way
- "BEFORE" toggle tab / "WITH FEDERATION" toggle tab
- Cards animate between states with active tab changing colors and showing "Enhanced" badges

#### AgentsSection (`#agents`)
- 7 agent cards in a 3-column grid
- Each card: gradient icon box, name, role title, description, "Ready" status indicator with green pulse
- Hover effect: lift + shadow + subtle gradient overlay
- Bottom visualization: Overlapping agent circles → "coordinated by" → Orchestrator circle with ring

#### CapabilitiesSection (`#capabilities`, `bg-muted/30`)
- 3-tab switcher: Proposal Generation, Client Intelligence, Knowledge Discovery
- Left side: Tab content with icon, title, description, and 3 feature items (icon + title + stat badge + description)
- Right side: Mock UI preview card with macOS-style window chrome (red/yellow/green dots)
  - Proposals tab: Shows chat interaction → progress bars → "Proposal Generated" card
  - Research tab: Shows briefing card with company stats
  - Knowledge tab: Shows 3 match results with percentages

#### RoadmapSection (`#implementation`)
- 4-phase timeline with alternating left/right layout
- Vertical connecting line (gradient, desktop only)
- Each phase: badge (Phase 1–4), title, 3 feature bullets with CheckCircle2 icons
- Phase 1 has "completed" status (green styling), Phases 2–4 are "planned"
- Large phase numbers as watermarks (text-8xl, text-muted/30)
- Phase icons in gradient circle nodes on the timeline

#### FooterSection
- CTA section: "We're not building IT infrastructure. We're building core IP amplification." + button
- Footer links: Platform links + Try It links (to app pages)
- Brand section with "F" logo, description, "Demo environment" indicator
- Bottom bar: Copyright + "Proof of Concept" notice

### 6.2 Chat Page (`/chat`)

The primary interface. Two-panel layout within the main content area.

**Left Panel** (w-64, border-r, `hidden md:flex`):
- Header: "Conversations" title + Plus button to create new conversation
- `ConversationList` component: Scrollable list of conversations
  - Each item: Title (truncated), message count + date
  - Active conversation highlighted with `bg-primary/10 text-primary`
  - Empty state: MessageSquare icon + "No conversations yet"

**Right Panel** (flex-1):
- **Header** (h-14): Shows active conversation title or "New Chat"
- **Messages area** (ScrollArea, flex-1, p-4):
  - Loading state: Skeleton bubbles
  - Empty state: "Welcome to Federation / Start a conversation with our AI agents"
  - Message bubbles:
    - User messages: Right-aligned, `bg-primary text-primary-foreground`, Avatar "U"
    - Assistant messages: Left-aligned, `bg-muted text-foreground`, Avatar "F"
    - Each bubble shows timestamp
    - Max width: `max-w-[85%]`
  - Loading indicator: "F" avatar + "Agents are working..." with spinner
- **Input area** (border-t, p-4):
  - Textarea (auto-expanding, `min-h-[44px] max-h-32`, resize-none)
  - Send button (icon-only)
  - Helper text: "Press Enter to send, Shift+Enter for new line"
  - Enter sends, Shift+Enter creates new line
  - Disabled while loading

**Behavior**:
- Optimistic updates: User message appears immediately in UI before API response
- Auto-scrolls to bottom on new messages
- Creates new conversation on first message if none active (uses first 50 chars as title)
- WebSocket connection established per conversation
- On orchestrator `agent.completed` WebSocket event → refetch messages after 100ms delay
- Conversations fetched via React Query, synced to Zustand store

### 6.3 Proposals Page (`/proposals`)

**Header** (h-14): "Proposals" title + "Generate Proposal" button

**Content**:
- Loading: Centered spinner
- Empty state: FileText icon + "No proposals yet" + "Generate your first proposal using AI agents"
- Grid: `md:grid-cols-2 lg:grid-cols-3` with ProposalCard components

**ProposalCard**:
- FileText icon + title + format badge
- Creation date
- Content preview (200 chars, line-clamp-3)
- "View" link button + "Export" button (calls `documentsApi.export(id, "pdf")`)

**GenerateProposalDialog** (opened by "Generate Proposal" button):
- Form fields:
  - Client Name (required) + Industry (required) — 2-column row
  - Engagement Type (required) — full width
  - Scope Description (required) — Textarea
  - Budget Range + Timeline — 2-column row
  - Additional Context — Textarea
- Cancel + Generate buttons
- On success: Invalidates proposals query, closes dialog, resets form
- Loading state: Spinner on Generate button

### 6.4 Research Page (`/research`)

**Header** (h-14): "Research" title

**Content**: Two tabs — "Research Query" and "Client Briefing"

**Research Query Tab**:
- Card with Search icon, title, description
- Textarea input (`min-h-32`) with placeholder
- "Start Research" button + source badges (Web, News, Company Data)
- Result card (`bg-muted/50`) showing status + message

**Client Briefing Tab**:
- Card with Building2 icon, title, description
- Company Name input field
- "Generate Briefing" button
- Result card showing company_name + status + message

Both tabs use `useMutation` from React Query.

### 6.5 Knowledge Page (`/knowledge`)

**Header** (h-14): "Knowledge Base" title

**Search bar** (border-b, p-4):
- Search icon inside input + text input + Search button + Clear button (shown when results exist)
- Enter key triggers search

**Content** (ScrollArea):
- Loading: Centered spinner
- Empty: BookOpen icon + "No knowledge items found"
- Results count text when search results shown
- Grid: `md:grid-cols-2 lg:grid-cols-3` of KnowledgeCard components

**KnowledgeCard**:
- Title + relevance score badge (percentage)
- Category badge + Industry badge (optional)
- Content preview (line-clamp-4)
- Tags (first 5, each with Tag icon)

**Data flow**: Initial load fetches all knowledge items via `knowledgeApi.list()`. Search replaces displayed items with `knowledgeApi.search()` results. Clear resets to full list.

### 6.6 Analytics Page (`/analytics`)

**Header** (h-14): "Analytics" title

**Content** (ScrollArea):

**Metrics Cards** (4-column grid on lg):
| Card | Icon | Value | Description |
|---|---|---|---|
| Total Executions | `Activity` | `metrics.total_executions` | "In the last 24 hours" |
| Active Agents | `BarChart3` | `metrics.agent_stats.length` | "Agents with activity" |
| Total Tokens | `Zap` | Calculated sum | "Tokens used today" |
| Avg Response Time | `Clock` | Calculated average in seconds | "Average task duration" |

**Agent Performance Card**:
- Horizontal bar chart per agent
- Each row: Colored dot + agent name (capitalize) + progress bar (proportional to total) + execution count + token count

**Recent Traces Card**:
- 3-tab filter: All / Completed / Failed
- Each trace row: Agent color dot + agent name + task type + timestamp + duration + token count + status badge
- Status badge colors: Same as agent status panel
- Shows first 20 traces per tab

---

## 7. State Management (Zustand Store)

### Store Shape

```typescript
interface FederationStore {
  // Conversations
  conversations: Conversation[];
  activeConversationId: string | null;
  setConversations, addConversation, setActiveConversation, updateConversation

  // Messages (keyed by conversation ID)
  messages: Record<string, Message[]>;
  setMessages, addMessage, appendToLastMessage

  // Agent States (keyed by AgentName)
  agentStates: Record<AgentName, AgentState>;
  updateAgentStatus, resetAgentStates

  // Documents
  documents: Document[];
  setDocuments, addDocument

  // UI
  isSidebarOpen: boolean;        toggleSidebar()
  isLoading: boolean;            setLoading()
  error: string | null;          setError()
  isStreaming: boolean;           
  streamingMessageId: string | null;  setStreaming()
}
```

### Key Patterns
- Use `devtools` and `subscribeWithSelector` middleware
- Export a stable empty array constant (`EMPTY_MESSAGES`) for default message state — prevents infinite re-renders
- Initialize all 7 agents in `idle` state on store creation
- `appendToLastMessage` enables real-time token streaming — appends to the last message's content
- Export selectors: `selectActiveConversation`, `selectActiveMessages`, `selectActiveAgents`
- Export `useShallow` from `zustand/react/shallow` for component use

---

## 8. API Client (`src/lib/api.ts`)

### Base Configuration
- `API_BASE` from `NEXT_PUBLIC_API_URL` env var, defaults to `http://localhost:8000`
- Generic `fetchJson<T>` helper that sets `Content-Type: application/json`, parses response, throws `ApiError` with status code on non-ok responses

### API Namespaces

**chatApi**:
- `listConversations(limit?, offset?)` → `GET /api/chat/conversations`
- `createConversation(data)` → `POST /api/chat/conversations`
- `getConversation(id)` → `GET /api/chat/conversations/{id}`
- `listMessages(conversationId, limit?, offset?)` → `GET /api/chat/conversations/{id}/messages`
- `sendMessage(conversationId, data)` → `POST /api/chat/conversations/{id}/messages`

**proposalsApi**:
- `list(limit?, offset?)` → `GET /api/proposals`
- `get(id)` → `GET /api/proposals/{id}`
- `generate(data)` → `POST /api/proposals/generate`

**researchApi**:
- `query(data)` → `POST /api/research/query`
- `briefing(data)` → `POST /api/research/briefing`

**documentsApi**:
- `list(docType?, limit?, offset?)` → `GET /api/documents`
- `get(id)` → `GET /api/documents/{id}`
- `export(id, format)` → `POST /api/documents/{id}/export` (uses raw `fetch`, not `fetchJson`, since response is a file download)

**knowledgeApi**:
- `search(data)` → `POST /api/knowledge/search`
- `list(category?, limit?, offset?)` → `GET /api/knowledge` (Note: this endpoint may need a fallback to search with empty query if the backend doesn't have a list endpoint)
- `get(id)` → `GET /api/knowledge/{id}`

**analyticsApi**:
- `traces(agentName?, status?, limit?, offset?)` → `GET /api/analytics/traces`
- `metrics(period?)` → `GET /api/analytics/metrics`

---

## 9. WebSocket Client (`src/lib/websocket.ts`)

### AgentWebSocket Class (Singleton)

Manages a single WebSocket connection per conversation.

**Connection URL**: `${WS_BASE}/ws/agents/${conversationId}` where `WS_BASE` defaults to `ws://localhost:8000`.

**Event handling**: Parses incoming JSON as `WSEvent`, dispatches by `event_type`:

| Event | Store Action |
|---|---|
| `agent.started` | `updateAgentStatus(name, "executing", task)` |
| `agent.thinking` | `updateAgentStatus(name, "thinking", thought)` |
| `agent.completed` | `updateAgentStatus(name, "completed")` |
| `agent.handoff` | Set `from_agent` to `idle` (or `waiting` if orchestrator), set `to_agent` to `waiting` |
| `agent.error` | `updateAgentStatus(name, "error")` |
| `stream.token` | `appendToLastMessage(activeConversationId, token)` |
| `document.generated` | Trigger handler callback |

**Reconnection**: Exponential backoff (1s base, max 30s), up to 5 attempts.

**Lifecycle**: `connect()` disconnects existing connection first. `disconnect()` clears timeout, closes socket, resets agent states.

### useAgentWebSocket Hook

React hook that:
- Connects when `conversationId` is non-null
- Forwards handler callbacks via `useRef` to avoid stale closures
- Does NOT disconnect on cleanup (persists across re-renders)
- Returns `{ isConnected, disconnect }`

---

## 10. TypeScript Types (`src/lib/types.ts`)

Define these types to match the backend Pydantic schemas exactly:

```typescript
// Chat
interface Message { id, conversation_id, role: "user"|"assistant"|"system", content, created_at, metadata }
interface MessageCreate { content, metadata? }
interface Conversation { id, title: string|null, created_at, updated_at, metadata, message_count }
interface ConversationCreate { title?, metadata? }

// Agents
type AgentName = "orchestrator"|"strategist"|"researcher"|"analyst"|"scribe"|"advisor"|"memory"
type AgentStatusType = "idle"|"thinking"|"executing"|"waiting"|"completed"|"error"
interface AgentStatus { agent_id, agent_type: AgentName, status: AgentStatusType, current_task, progress, last_activity }
interface AgentTrace { id, agent_name: AgentName, task_type, status, started_at, completed_at, tokens_used, error }

// Documents
interface Document { id, title, doc_type, content, format, created_at, metadata }

// Knowledge
interface KnowledgeItem { id, title, content, category, industry, tags: string[], score? }
interface KnowledgeSearchRequest { query, category?, industry?, limit? }

// Research
interface ResearchRequest { query, research_type?: "comprehensive"|"quick"|"deep", sources? }
interface BriefingRequest { company_name, industry?, focus_areas? }

// Proposals
interface ProposalRequest { client_name, client_industry, engagement_type, scope_description, budget_range?, timeline?, additional_context? }

// WebSocket Events
type WSEventType = "agent.started"|"agent.thinking"|"agent.completed"|"agent.handoff"|"agent.error"|"stream.token"|"document.generated"|"connection.established"|"connection.error"
interface WSEvent<T = unknown> { event_type: WSEventType, timestamp: string, data: T }
interface AgentStartedEvent { agent_name: AgentName, task: string }
interface AgentThinkingEvent { agent_name: AgentName, thought: string }
interface AgentCompletedEvent { agent_name: AgentName, result: string, tokens_used?: number }
interface AgentHandoffEvent { from_agent: AgentName, to_agent: AgentName, context: string }
interface StreamTokenEvent { token: string, agent_name: AgentName }
interface DocumentGeneratedEvent { document_id: string, doc_type: string, title: string }

// Analytics
interface AgentStats { agent: AgentName, executions: number, avg_tokens: number }
interface Metrics { period: string, since: string, agent_stats: AgentStats[], total_executions: number }
```

---

## 11. shadcn/ui Components to Install

Run these commands to install the required primitives:

```bash
npx shadcn@latest add avatar badge button card dialog dropdown-menu input scroll-area separator sheet skeleton tabs textarea tooltip
```

---

## 12. Playwright E2E Tests

Create a `frontend/e2e/` directory with Playwright test files. Install Playwright:

```bash
cd frontend
pnpm add -D @playwright/test
npx playwright install
```

Create `playwright.config.ts` in `frontend/`:
```typescript
import { defineConfig } from '@playwright/test';

export default defineConfig({
  testDir: './e2e',
  baseURL: 'http://localhost:3000',
  webServer: {
    command: 'pnpm dev',
    port: 3000,
    reuseExistingServer: true,
  },
  use: {
    headless: true,
    screenshot: 'only-on-failure',
  },
});
```

### Tests to Write

#### `e2e/landing.spec.ts`
- Landing page loads with hero headline visible
- Navigation links are visible (Platform, Agents, Capabilities, Implementation)
- "Launch App" button navigates to `/chat`
- All 7 agents are displayed in the agents section
- Theme toggle switches between light and dark mode
- Sample prompt buttons are clickable
- Before/After toggle in paradigm shift section works
- Capabilities tabs switch content (Proposals, Client Intelligence, Knowledge Discovery)

#### `e2e/chat.spec.ts`
- Chat page loads with sidebar and conversation panel visible
- "New Chat" header is shown when no conversation is active
- "Welcome to Federation" empty state is displayed
- Creating a new conversation shows it in the conversation list
- Typing a message and pressing Enter sends it (shows optimistic user bubble)
- Send button is disabled when input is empty or loading
- Shift+Enter creates a new line instead of sending
- Conversation list shows conversation titles

#### `e2e/proposals.spec.ts`
- Proposals page loads with "Proposals" header
- Empty state shows "No proposals yet" message
- "Generate Proposal" button opens dialog
- Dialog form has all required fields (Client Name, Industry, Engagement Type, Scope)
- Form validation prevents submission without required fields
- Cancel button closes the dialog

#### `e2e/research.spec.ts`
- Research page loads with two tabs visible
- "Research Query" tab shows textarea and "Start Research" button
- "Client Briefing" tab shows company name input and "Generate Briefing" button
- Source badges (Web, News, Company Data) are visible
- Start Research button is disabled when query is empty

#### `e2e/knowledge.spec.ts`
- Knowledge page loads with search bar
- Empty state message shown when no items
- Search input and button are functional
- Clear button appears after search results
- Enter key triggers search

#### `e2e/analytics.spec.ts`
- Analytics page loads with metrics cards (4 cards visible)
- Metrics cards show: Total Executions, Active Agents, Total Tokens, Avg Response Time
- Agent Performance section is visible
- Recent Traces section has tabs: All, Completed, Failed
- Tab switching filters the trace list

#### `e2e/navigation.spec.ts`
- Sidebar navigation links work correctly
- Active page is highlighted in sidebar
- All 5 nav links navigate to correct pages (/chat, /proposals, /research, /knowledge, /analytics)
- Logo links back to landing page (`/`)
- Mobile sidebar toggle shows/hides on narrow viewports

---

## 13. Build & Run Commands

```bash
# Install dependencies
cd frontend && pnpm install

# Development server
pnpm dev

# Production build
pnpm build

# Start production
pnpm start

# Lint
pnpm lint

# Run E2E tests
pnpm exec playwright test

# Run E2E tests with UI
pnpm exec playwright test --ui
```

---

## 14. Environment Variables

```env
NEXT_PUBLIC_API_URL=http://localhost:8000
NEXT_PUBLIC_WS_URL=ws://localhost:8000
```

---

## 15. Critical Implementation Notes

1. **Server Components by default** — Only add `'use client'` when the component uses hooks, event handlers, or browser APIs. Layouts and page.tsx files should be Server Components when possible.

2. **No `any` types** — TypeScript strict mode. All data is typed.

3. **React Query for server state, Zustand for client state** — Don't duplicate. React Query handles fetching/caching from the API. Zustand handles UI state (active conversation, sidebar toggle, agent statuses).

4. **Optimistic updates in chat** — User messages appear immediately in the UI before the API responds. The API response updates the store after.

5. **WebSocket integration** — The WebSocket updates agent states in real-time via the Zustand store. Components that render agent states reactively update.

6. **Date formatting** — Use `toLocaleDateString('en-US', { timeZone: 'America/New_York' })` and `toLocaleTimeString('en-US', { timeZone: 'America/New_York' })` consistently.

7. **No `useEffect` for data sync except** the conversation/message sync between React Query and Zustand (necessary because the chat interface uses both).

8. **Responsive design** — Sidebar hidden on mobile with toggle. Agent panel hidden below `xl`. Chat conversation list hidden below `md`. Landing page fully responsive.

9. **Accessibility** — All interactive elements must be keyboard accessible. Use semantic HTML. Images need alt text. shadcn/ui components handle ARIA attributes.

10. **Don't mock the backend** — The API client should make real HTTP calls to `localhost:8000`. For Playwright tests, you may need to mock API responses at the network level using Playwright's route interception.

