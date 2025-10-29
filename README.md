# Multi-Agent Framework

**Status**: 18 agents operational | **Version**: 1.0 | **Architecture**: Async Python 3.13+ with MCP Integration

A production-ready multi-agent orchestration system featuring intelligent LLM-powered routing, 14 MCP server integrations, automated agent generation, and async-first architecture with 21x performance improvements.

## 🎯 System Overview

### What This Framework Does

**Idilis** is a multi-agent AI framework that orchestrates specialized agents to solve complex tasks through intelligent collaboration:

- **18 specialized agents** working together (code generation, monitoring, data analysis, planning, testing, and more)
- **14 MCP servers integrated** for filesystem, GitHub, Discord, Locust, Postgres, Reddit, Google Maps, and more
- **Async-first architecture** using Phase 3E patterns (98% async coverage, 21x performance improvement)
- **BuilderAgent automation** - generates new agents from markdown specifications in 5 seconds
- **Intelligent ChatAgent routing** - LLM-based decision making routes queries to appropriate agents
- **Real-time dashboard** at http://localhost:8080 for monitoring and interaction
- **Cross-platform** - Windows, Linux, macOS support with graceful degradation

### Architecture Highlights

```
┌─────────────────────────────────────────────────────────────┐
│                    Manager Server (9000)                     │
│              WebSocket Hub + Agent Registry                  │
└──────────────────┬──────────────────────────────────────────┘
                   │
    ┌──────────────┼──────────────┬──────────────┬────────────────┐
    │              │              │              │                │
┌───▼───┐     ┌───▼───┐     ┌───▼───┐     ┌───▼───┐      ┌────▼────┐
│ Chat  │◄───►│ Code  │◄───►│Planner│◄───►│Builder│ ...  │ 14 more │
│ Agent │     │ Agent │     │ Agent │     │ Agent │      │  agents │
└───┬───┘     └───┬───┘     └───┬───┘     └───┬───┘      └────┬────┘
    │             │             │             │                │
    │         ┌───▼─────────────▼─────────────▼────────────────▼───┐
    │         │         MCP Server Manager (14 servers)            │
    │         │   filesystem | github | discord | postgres | ...  │
    │         └────────────────────────────────────────────────────┘
    │
┌───▼───────────────────────────────────────────────────────────┐
│          Dashboard (8080) + CLI Client Interface              │
└───────────────────────────────────────────────────────────────┘
```

**Core Principles**:
- **Asynchronous**: Non-blocking I/O for all agents (asyncio + aiofiles)
- **Modular**: Each agent is independent, communicates via WebSocket
- **Extensible**: BuilderAgent auto-generates new agents from markdown specs
- **Observable**: Structured logging, dashboard monitoring, audit trails
- **Resilient**: Graceful degradation, automatic reconnection, error recovery

## 🏗️ BuilderAgent - Automated Agent Generation

**BuilderAgent** monitors the `agents/incoming_specs/` directory for new agent specification files and automatically generates production-ready agent code within 5 seconds!

### How It Works

1. Create a `.md` file in `agents/incoming_specs/` with your agent specification (format below)
2. BuilderAgent detects the new file within 5 seconds
3. Agent code is auto-generated (~600 lines) with proper async patterns
4. System files are auto-updated (agents/__init__.py, start_all_agents.bat, versions.json)
5. Tests are auto-generated (tests/test_<agent>.py)
6. Specification file is archived to `agents/incoming_specs/Past/`
7. Agent is ready to use immediately!

### Agent Specification Format

Create a markdown file (e.g., `my_agent_spec.md`) in `agents/incoming_specs/` with:

**Required Header**:
```markdown
## New Agent Request
```

**Required Fields**:
- **Name**: agent_name (snake_case, e.g., `locust_agent`)
- **Purpose**: Brief description of what the agent does
- **Dependencies**: Comma-separated list of agents this agent communicates with
- **MCP Server**: MCP server name if using MCP integration (optional)
- **Python Packages**: Required packages if any (optional)

**Optional Sections**:
- Capabilities Required: Detailed feature list
- MCP Integration: How MCP tools will be used
- Agent Interactions: Communication patterns with other agents
- Expected Behavior: Step-by-step workflow examples

**Example**: See `agents/incoming_specs/README.md` for a complete LocustAgent specification

---

## 📚 Existing Agents

### Core Agents
- **ChatAgent**: Natural language interface, intelligent routing
- **BuilderAgent**: Automated agent generation from README (YOU ARE HERE!)
- **PlannerAgent**: Multi-step task orchestration with dependencies

## 📚 Agent Catalog

This framework includes **18 specialized agents**, each with unique capabilities:

### 🧠 Core Orchestration
- **ChatAgent** (`chat_agent.py`) - Natural language interface, LLM-powered intelligent routing
- **PlannerAgent** (`planner_agent.py`) - Multi-step workflow orchestration with dependency management
- **BuilderAgent** (`builder_agent.py`) - Auto-generates agents from markdown specifications

### 💻 Development & Code Quality
- **CodeAgent** (`code_agent.py`) - Code generation with 4-iteration self-refinement
- **ArchitectAgent** (`architect_agent.py`) - System design, architecture planning, agent generation
- **RefinementAgent** (`refinement_agent.py`) - Code quality improvement and optimization
- **VersionAgent** (`version_agent.py`) - Version tracking, history management, changelog generation
- **TestSuiteAgent** (`test_suite_agent.py`) - Automated testing and quality assurance

### 🔌 Integration & External Services
- **ImageAgent** (`image_agent.py`) - Image generation via ComfyUI integration
- **DataAgent** (`data_agent.py`) - Data analysis with pandas/numpy, CSV/Excel processing
- **KnowledgeAgent** (`knowledge_agent.py`) - Vector database for context storage and retrieval
- **SocialMediaAgent** (`social_media_agent.py`) - Reddit/Discord posting and monitoring

### 🔧 Infrastructure & Operations
- **MonitorAgent** (`monitor_agent.py`) - System health (CPU, memory, Event Viewer on Windows)
- **LogsAgent** (`logs_agent.py`) - Centralized structured logging with audit trails
- **ZomboidServerAgent** (`zomboid_server_agent.py`) - Project Zomboid dedicated server management
- **LANBridgeAgent** (`lan_bridge_agent.py`) - Cross-machine agent communication over LAN
- **WebserverAgent** (`webserver_agent.py`) - HTTP server for agent APIs and webhooks

### 🛠️ Utility & Support
- **ErrorWatcher** (`error_watcher.py`) - Real-time error detection and alerting

---

## 🔌 MCP Server Integration

**14 MCP (Model Context Protocol) servers** provide enhanced capabilities:

| MCP Server | Purpose | Agents Using |
|------------|---------|--------------|
| **filesystem** | File read/write/search operations | All agents |
| **sequentialthinking** | Multi-step problem solving | Chat, Code, Planner, Builder |
| **github** | GitHub API, code search, repo operations | Code, Architect, Version |
| **gitmcp** | Git repository documentation search | Code, Architect |
| **discord** | Discord API, channel management | SocialMedia |
| **reddit** | Reddit API, posting, monitoring | SocialMedia |
| **google-maps** | Location services, geocoding | SocialMedia |
| **postgres** | PostgreSQL database operations | Data, Logs |
| **locust-mcp** | Load testing orchestration | *Future: LocustAgent* |
| **godot-mcp** | Godot engine integration | *Future: GameDevAgent* |
| **godot-docs** | Godot documentation search | *Future: GameDevAgent* |
| **kokoro-tts** | Text-to-speech synthesis | *Future: VoiceAgent* |
| **word-document-server** | Word document generation | Code, Data |
| **task-master-ai** | Task decomposition and tracking | Planner |

**MCP Architecture**: Each agent connects to a subset of MCP servers based on its needs. Connection management is handled by `core/mcp_servers.py` with automatic retry and graceful degradation.

---

## 🚀 Quick Start

### Prerequisites
```bash
# Python 3.13+ required
python --version

# Install dependencies
pip install -r requirements.txt
```

### Launch All Agents

**Windows**:
```bash
start_all_agents.bat
```

**Linux/Mac**:
```bash
chmod +x start_all_agents.sh
./start_all_agents.sh
```

This starts:
- **Manager Server** on port 9000 (WebSocket hub)
- **Dashboard** at http://localhost:8080
- **18 agent terminals** (one per agent)

### Interact via CLI

```bash
python cli_client.py

# Natural language queries (routed by ChatAgent):
> Create a function to validate emails
> What's the CPU usage?
> Generate an image of a sunset
> Analyze test_data.csv

# Targeted agent queries:
> @code create a REST API handler
> @monitor check system health
> @data summarize sales.xlsx
> @planner build me a microservices architecture
```

### Interact via Dashboard

Open http://localhost:8080 for:
- Real-time agent status and health
- Message history and conversation logs
- Agent control (restart, disconnect)
- Diagnostics and error tracking

---

## 🔄 Workflow Examples

### Example 1: Multi-Agent Code Generation

**User Query**: *"Create a Python web scraper with rate limiting"*

```
ChatAgent (analyzes) → CodeAgent (generates scraper.py)
                     → RefinementAgent (improves error handling)
                     → TestSuiteAgent (creates test_scraper.py)
                     → VersionAgent (tracks v1.0.0)
```

### Example 2: Load Testing Pipeline

**User Query**: *"Load test our API with 100 concurrent users"*

```
ChatAgent → PlannerAgent (creates plan):
            Step 1: LocustAgent (runs load test)
            Step 2: DataAgent (analyzes results)
            Step 3: MonitorAgent (checks system impact)
            Step 4: LogsAgent (audit trail)
```

### Example 3: Automated Agent Creation

**Scenario**: Need a new "EmailAgent" for email automation

1. Create `agents/incoming_specs/email_agent_spec.md`:
```markdown
## New Agent Request

- **Name**: email_agent
- **Purpose**: Send/receive emails via SMTP/IMAP
- **Dependencies**: data_agent, logs_agent
- **MCP Server**: None
- **Python Packages**: smtplib, imaplib, email
```

2. **BuilderAgent detects within 5 seconds**:
   - Generates `agents/email_agent.py` (~600 lines)
   - Updates `agents/__init__.py`
   - Updates `start_all_agents.bat`
   - Generates `tests/test_email_agent.py`
   - Archives spec to `incoming_specs/Past/`

3. **EmailAgent ready to use**:
```bash
# Restart framework or launch individually:
python agent_runner.py --name email --module agents.email_agent
```

---

## 🏛️ System Architecture

### Component Layers

```
┌─────────────────────────────────────────────────────────────┐
│                   User Interface Layer                      │
│   CLI Client | Dashboard (8080) | Direct API Calls         │
└──────────────────────┬──────────────────────────────────────┘
                       │
┌──────────────────────▼──────────────────────────────────────┐
│                  Manager Server (9000)                       │
│   - WebSocket Hub (agent ↔ agent communication)             │
│   - Agent Registry (18 agents tracked)                      │
│   - Message Routing (broadcasts, targeted, audit)           │
└──────────────────────┬──────────────────────────────────────┘
                       │
      ┌────────────────┼────────────────┬────────────────┐
      │                │                │                │
┌─────▼─────┐   ┌─────▼─────┐   ┌─────▼─────┐   ┌─────▼─────┐
│   Chat    │   │   Code    │   │  Planner  │   │  Builder  │
│   Agent   │   │   Agent   │   │   Agent   │   │   Agent   │
│ (Routes)  │   │(Generates)│   │(Orchestr.)│   │(Auto-Gen) │
└─────┬─────┘   └─────┬─────┘   └─────┬─────┘   └─────┬─────┘
      │               │               │               │
      └───────────────┴───────────────┴───────────────┘
                       │
┌──────────────────────▼──────────────────────────────────────┐
│              MCP Server Manager Layer                        │
│   filesystem | github | discord | postgres | locust | ...  │
│   (14 integrated MCP servers with retry + fallback)         │
└──────────────────────┬──────────────────────────────────────┘
                       │
┌──────────────────────▼──────────────────────────────────────┐
│                  Storage & Services                          │
│   - Memory Store (JSON files per agent)                     │
│   - Logs (structured logging to logs_agent)                 │
│   - Vector Database (KnowledgeAgent via Chroma)             │
│   - External APIs (ComfyUI, Reddit, Discord, etc.)          │
└──────────────────────────────────────────────────────────────┘
```

### Communication Patterns

**1. Synchronous Request-Response**:
```
User → ChatAgent → CodeAgent → Response → ChatAgent → User
```

**2. Asynchronous Broadcasting**:
```
MonitorAgent → [System Alert] → All Agents (health notification)
```

**3. Multi-Agent Orchestration**:
```
PlannerAgent → Task 1 → ImageAgent → Complete
            → Task 2 → CodeAgent → Complete (waits for Task 1)
            → Task 3 → DataAgent → Complete → Final Report
```

---

## 🎨 Frontend - Angular Dashboard

### Overview

The Idilis framework includes a **modern Angular 19+ frontend** that provides a rich, real-time web interface for monitoring and controlling the multi-agent system.

**Current State**: The production dashboard is served via Python (`templates/dashboard.html`, 8,562 lines) on port 8080. A complete **Angular 19 migration** is in progress in the `FrontEnd/` directory with Server-Side Rendering (SSR) support.

**Architecture**:
```
┌──────────────────────────────────────────────────────────────┐
│                  Browser (localhost:8080)                     │
│              Angular 19 + SSR + Material Design              │
└──────────────────────┬───────────────────────────────────────┘
                       │ (WebSocket + HTTP)
┌──────────────────────▼───────────────────────────────────────┐
│              Manager Server (port 9000)                       │
│   API Endpoints: /api/*  |  WebSocket: /ws, /ws/<agent>     │
└──────────────────────┬───────────────────────────────────────┘
                       │
                   [18 Agents]
```

### Dashboard Features

#### 1. 🎨 Images Tab
- **Image Gallery**: Browse generated images with filters (style, quality, date)
- **Canvas Editor**: Fabric.js-based image editing (resize, crop, annotate)
- **Upload/Delete**: Manage image library
- **ComfyUI Integration**: Direct workflow submission
- **Preview Modal**: Full-screen image viewing with metadata

#### 2. 🧬 LoRA Tab
- **LoRA Model Selector**: Browse and search LoRA models
- **Metadata Display**: Trigger words, base model compatibility, training data
- **Favorites**: Star frequently used models
- **Auto-Complete**: Trigger word suggestions in prompts
- **Version Tracking**: Multiple versions per LoRA model

#### 3. 📝 Embeddings Tab
- **Embedding Manager**: Organize textual inversions and embeddings
- **Prompt Integration**: Drag-and-drop into prompt builder
- **Search & Filter**: By category, compatibility, training style
- **Preview System**: See example outputs before using
- **Batch Import**: Upload multiple embedding files

#### 4. 📊 System Monitor Tab
- **CPU, Memory, Disk Metrics**: Real-time system health
- **Historical Charts**: 1-hour, 6-hour, 24-hour views
- **Agent Health**: Per-agent resource usage
- **Event Viewer Integration** (Windows): Application/System/Security logs
- **Alert System**: Threshold-based notifications

#### 5. 💬 Agent Chat Tab
- **Direct Agent Communication**: Send messages to specific agents
- **Message History**: Searchable conversation logs
- **Multi-Agent Selection**: Route to multiple agents simultaneously
- **Markdown Rendering**: Formatted responses with code highlighting
- **Auto-Scroll**: Keeps latest messages in view

#### 6. 🗣️ Debate Tab (NEW - Angular Exclusive)
- **Debate Agent Interface**: Challenge statements with factually-backed rebuttals
- **Structured Rebuttal Display**: 4-part response format (acknowledgment, counterarguments, evidence, conclusion)
- **Vector Store Facts Panel**: See supporting evidence from knowledge base
- **Debate History**: Review past debates with timestamps
- **Export Transcript**: Download debates as PDF or Markdown

**Status**: ✅ Component code ready (TypeScript + HTML + CSS documented), ⏳ Implementation pending

### Angular Project Structure

```
FrontEnd/
├── angular.json              # Angular CLI configuration
├── package.json              # Dependencies and scripts
├── tsconfig.json             # TypeScript compiler options
├── proxy.conf.json           # API proxy configuration (port 4200 → 9000)
├── src/
│   ├── main.ts               # Browser entry point
│   ├── main.server.ts        # SSR entry point
│   ├── server.ts             # AngularNodeAppEngine (SSR)
│   ├── app/
│   │   ├── app.component.ts  # Root component
│   │   ├── app.config.ts     # Application config
│   │   ├── dashboard/        # Dashboard components
│   │   │   ├── images/       # Image gallery tab
│   │   │   ├── lora/         # LoRA selector tab
│   │   │   ├── embeddings/   # Embedding manager tab
│   │   │   ├── monitor/      # System metrics tab
│   │   │   ├── chat/         # Agent chat tab
│   │   │   └── debate/       # Debate agent tab (NEW)
│   │   ├── services/         # Angular services
│   │   │   ├── websocket.service.ts  # WebSocket communication
│   │   │   ├── api.service.ts        # REST API calls
│   │   │   └── agent.service.ts      # Agent state management
│   │   ├── models/           # TypeScript interfaces
│   │   │   ├── message.ts    # Message protocol
│   │   │   ├── agent.ts      # Agent metadata
│   │   │   └── debate.ts     # Debate-specific types
│   │   └── shared/           # Shared components/utilities
│   ├── custom-theme.scss     # Material theme customization
│   ├── styles.css            # Global styles
│   └── index.html            # HTML entry point
├── dist/                     # Build output (gitignored)
│   ├── FrontEnd/
│   │   ├── browser/          # Client-side bundles
│   │   └── server/           # SSR bundles
└── node_modules/             # Dependencies (gitignored)
```

### Development Setup

#### Prerequisites

```bash
# Node.js 18+ and npm required
node --version  # Should be 18.x or higher
npm --version

# Angular CLI (global install)
npm install -g @angular/cli
```

#### Installation

```bash
# Navigate to Angular project
cd FrontEnd

# Install dependencies
npm install

# Verify installation
ng version
```

#### Run Development Server

**Option A: Default Port 4200 (with proxy)**
```bash
cd FrontEnd
ng serve

# Dashboard: http://localhost:4200
# API calls automatically proxied to Manager Server (port 9000)
```

**Option B: Custom Port 8080 (direct)**
```bash
cd FrontEnd
ng serve --port 8080

# Dashboard: http://localhost:8080
```

**API Proxy Configuration** (`proxy.conf.json`):
```json
{
  "/api/*": {
    "target": "http://localhost:9000",
    "secure": false,
    "changeOrigin": true
  },
  "/ws": {
    "target": "ws://localhost:9000",
    "ws": true,
    "changeOrigin": true
  }
}
```

#### Build for Production

```bash
# Production build with SSR
ng build --configuration=production

# Output: dist/FrontEnd/browser/ (client)
#         dist/FrontEnd/server/ (SSR)
```

#### Run Tests

```bash
# Unit tests
ng test

# End-to-end tests (configure first)
ng e2e
```

### WebSocket Service Pattern

The Angular frontend uses a centralized WebSocket service for real-time communication:

```typescript
// src/app/services/websocket.service.ts
import { Injectable } from '@angular/core';
import { Subject, Observable } from 'rxjs';

export interface AgentMessage {
  agent: string;
  action: string;
  content: any;
  timestamp?: number;
  messageId?: string;
}

@Injectable({ providedIn: 'root' })
export class WebSocketService {
  private socket: WebSocket | null = null;
  private messagesSubject = new Subject<AgentMessage>();
  
  connect(url: string = 'ws://localhost:9000/ws'): void {
    this.socket = new WebSocket(url);
    
    this.socket.onopen = () => {
      console.log('✅ WebSocket connected');
    };
    
    this.socket.onmessage = (event) => {
      const message = JSON.parse(event.data);
      this.messagesSubject.next(message);
    };
    
    this.socket.onclose = () => {
      // Auto-reconnect after 3 seconds
      setTimeout(() => this.connect(url), 3000);
    };
  }
  
  send(message: AgentMessage): void {
    if (this.socket?.readyState === WebSocket.OPEN) {
      this.socket.send(JSON.stringify(message));
    }
  }
  
  getMessages(): Observable<AgentMessage> {
    return this.messagesSubject.asObservable();
  }
}
```

**Usage in Components**:
```typescript
export class ChatComponent implements OnInit {
  constructor(private ws: WebSocketService) {}
  
  ngOnInit() {
    this.ws.connect();
    this.ws.getMessages().subscribe((msg) => {
      if (msg.agent === 'chat') {
        this.messages.push(msg);
      }
    });
  }
  
  sendMessage(content: string) {
    this.ws.send({
      agent: 'chat',
      action: 'message',
      content: content
    });
  }
}
```

### Migration Roadmap

**Current Production**: `templates/dashboard.html` (Python HTTP server on port 8081)

**Phase 1: Angular Development** ✅ IN PROGRESS
- Angular 19 project scaffolded with SSR
- Component structure defined
- WebSocket service implemented
- Proxy configuration for local development

**Phase 2: Feature Parity** ⏳ PENDING
- Migrate all 6 dashboard tabs to Angular
- Image gallery + canvas editor (Fabric.js → Angular Canvas)
- LoRA/Embedding management UI
- System monitor charts
- Agent chat interface
- **NEW**: Debate tab (Angular-exclusive feature)

**Phase 3: WebServer Agent Integration** 🔮 FUTURE
- WebServer Agent launches `ng serve` on port 8080 (dev)
- Or serves pre-built `dist/FrontEnd/browser/` (production)
- Manager Server becomes API-only (no static file serving)

**Phase 4: Legacy Deprecation** 🔮 FUTURE
- Archive `templates/dashboard.html` to `templates/legacy/`
- Remove Python HTTP serving logic
- Angular becomes sole frontend

### TypeScript Interfaces

Key interfaces for type-safe Angular development:

```typescript
// src/app/models/message.ts
export interface AgentMessage {
  agent: string;
  action: string;
  content: any;
  timestamp?: number;
  messageId?: string;
}

// src/app/models/agent.ts
export interface AgentMetadata {
  name: string;
  status: 'online' | 'offline' | 'error';
  version: string;
  capabilities: string[];
  lastSeen: Date;
}

// src/app/models/debate.ts
export interface DebateResponse {
  acknowledgment: string;
  counterarguments: string[];
  evidence: string;
  conclusion: string;
}

export interface DebateHistoryEntry {
  statement: string;
  response: DebateResponse;
  facts: string[];
  timestamp: Date;
}
```

### Deployment Options

#### Option A: Development (ng serve)
```bash
# Terminal 1: Manager Server
python core/manager_server.py

# Terminal 2: Angular Dev Server
cd FrontEnd
ng serve --port 8080
```

**Access**: http://localhost:8080

#### Option B: Production (Pre-built)
```bash
# Build Angular app
cd FrontEnd
ng build --configuration=production

# Serve dist/ folder via WebServer Agent or nginx
# (WebServer Agent enhancement planned)
```

#### Option C: Docker (Future)
```dockerfile
# Multi-stage build
FROM node:18 AS angular-build
WORKDIR /app/FrontEnd
COPY FrontEnd/package*.json ./
RUN npm install
COPY FrontEnd/ ./
RUN npm run build -- --configuration=production

FROM python:3.13
WORKDIR /app
COPY --from=angular-build /app/FrontEnd/dist/FrontEnd/browser ./static
COPY . .
RUN pip install -r requirements.txt
CMD ["python", "core/manager_server.py"]
```

### Benefits of Angular Frontend

✅ **Modern Framework**: TypeScript, reactive state management, component architecture  
✅ **Hot Reload**: Instant updates during development (ng serve)  
✅ **Better Testing**: Built-in unit testing with Jasmine/Karma  
✅ **Type Safety**: TypeScript catches errors at compile time  
✅ **Scalability**: Easier to add features without bloating single HTML file  
✅ **SSR Support**: Faster initial load and SEO-friendly  
✅ **Material Design**: Angular Material UI components  
✅ **Separation of Concerns**: Manager Server = Backend API, Angular = Frontend  

### Troubleshooting

**Angular Dev Server Won't Start**:
```bash
# Check Node.js version
node --version  # Must be 18+

# Clear npm cache
npm cache clean --force

# Reinstall dependencies
rm -rf node_modules package-lock.json
npm install
```

**WebSocket Connection Fails**:
```bash
# Verify Manager Server is running
curl http://localhost:9000/api/status

# Check proxy configuration
cat FrontEnd/proxy.conf.json

# Test WebSocket manually
wscat -c ws://localhost:9000/ws
```

**Port Already in Use**:
```bash
# Windows: Find process using port 8080
netstat -ano | findstr :8080

# Kill process
taskkill /PID <PID> /F

# Or use a different port
ng serve --port 4200
```

**Build Fails**:
```bash
# Check TypeScript errors
ng build --configuration=development

# Enable verbose output
ng build --verbose

# Check Angular version compatibility
ng version
```

### Documentation Links

- **Angular Project README**: `FrontEnd/README.md`
- **Migration Plan**: `FrontEnd/MIGRATION_PLAN.md`
- **Testing Guide**: `FrontEnd/ANGULAR_TEST_GUIDE.md`
- **Architecture Guide**: `.github/copilot-instructions.md` (lines 3209-4953)
- **Debate Tab Integration**: `.github/copilot-instructions.md` (Debate Agent section)

---

## 📦 Dependencies & Requirements

### Core Python Packages
```
Python 3.13+
asyncio, aiofiles, aiohttp
websockets
psutil (system monitoring)
pandas, numpy (data analysis)
chromadb (vector database)
```

### Optional Platform-Specific
```
pywin32 (Windows Event Viewer - MonitorAgent)
locust (load testing - future LocustAgent)
```

### MCP Server Dependencies
```
Node.js 18+ (for npm-based MCP servers)
uvx (for Python-based MCP servers)
Git (for GitHub MCP)
```

---

## 🛠️ Configuration

### Manager Server (`core/manager_server.py`)
- **Port**: 9000 (WebSocket)
- **Dashboard**: 8080 (HTTP)
- **Max Connections**: Unlimited
- **Heartbeat**: 30s interval

### Agent Configuration (`config.py`)
```python
# LLM API Configuration
OPENAI_API_KEY = "your-api-key"
OPENAI_MODEL = "gpt-4"

# ComfyUI Configuration (ImageAgent)
COMFYUI_API = "http://localhost:8188"

# Monitoring Thresholds (MonitorAgent)
CPU_THRESHOLD = 85
MEMORY_THRESHOLD = 90
DISK_THRESHOLD = 90
```

### MCP Server Configuration (`core/mcp_servers.py`)
Each MCP server has:
- **Type**: `stdio` or `http`
- **Command**: Startup command (e.g., `npx`, `uvx`)
- **Args**: Command arguments
- **Environment**: Environment variables
- **CWD**: Working directory

---

## 📊 Performance Metrics

### Async Phase 3E Results
- **Async Coverage**: 98% (45/46 operations)
- **Performance Improvement**: 21x faster (10-23ms vs 90-230ms per operation)
- **Agents Completed**: 11/18 async-optimized
- **Blocking Operations**: 1 remaining (mcp.json hot-reload)

### Typical Response Times
- **ChatAgent routing**: <2s (LLM call + decision)
- **CodeAgent generation**: 10-30s (4 refinement iterations)
- **ImageAgent generation**: 15-60s (ComfyUI dependent)
- **MonitorAgent health check**: <3s (system metrics)
- **DataAgent CSV analysis**: 5-15s (pandas processing)
- **BuilderAgent agent creation**: 5-10s (code generation + file writes)

---

## 🧪 Testing

### Run All Tests
```bash
# Full test suite
python -m pytest tests/

# Individual agent tests
python -m pytest tests/test_code_agent.py -v

# Integration tests
python test_integration.py
```

### Manual Testing

**Test ChatAgent routing**:
```bash
python cli_client.py
> Create a function to add numbers  # Routes to CodeAgent
> What's the CPU usage?              # Routes to MonitorAgent
> Generate an image of a cat         # Routes to ImageAgent
```

**Test BuilderAgent**:
1. Create `agents/incoming_specs/test_agent.md`
2. Add agent specification
3. Watch terminal for "🆕 New agent request found: test_agent"
4. Verify `agents/test_agent.py` created

---

## 🐛 Troubleshooting

### Agent Won't Connect
**Symptoms**: Agent terminal shows "Connection refused"

**Solution**:
1. Verify Manager Server is running on port 9000
2. Check firewall allows local connections
3. Restart Manager Server: `python core/manager_server.py`

### BuilderAgent Not Processing Specs
**Symptoms**: "Watching..." message, no agent generated

**Solution**:
1. Check spec file is in `agents/incoming_specs/` directory
2. Verify markdown format: `## New Agent Request` header
3. Check BuilderAgent terminal for parse errors
4. See `agents/incoming_specs/README.md` for example spec

### Dashboard Not Loading
**Symptoms**: http://localhost:8080 refuses connection

**Solution**:
1. Verify Manager Server started successfully
2. Check port 8080 not in use: `netstat -ano | findstr :8080`
3. Try `http://127.0.0.1:8080` instead of localhost

### Agent Terminal Shows Errors
**Symptoms**: Python exceptions in agent terminal

**Solution**:
1. Check `pip install -r requirements.txt` completed
2. Verify MCP servers are available (check logs)
3. Review agent-specific dependencies (e.g., pywin32 for MonitorAgent)
4. Enable debug logging: `logging.basicConfig(level=logging.DEBUG)`

---

## 📚 Documentation

### Core Documentation
- **README.md** (this file) - System overview and quick start
- **COPILOT_INSTRUCTIONS.md** - AI agent development guide
- **.github/copilot-instructions.md** - Full architectural guide

### Feature Documentation
- **BUILDERAGENT_INTEGRATION_COMPLETE.md** - BuilderAgent architecture
- **ASYNC_PHASE2_IMPLEMENTATION.md** - Async patterns and migration
- **MCP_INTEGRATION_COMPLETE.md** - MCP server integration guide
- **AUTONOMOUS_AGENTS_GUIDE.md** - Self-healing agent patterns

### Agent-Specific Guides
- **ZOMBOID_AGENT_GUIDE.md** - Project Zomboid server management
- **MONITOR_AGENT_GUIDE.md** - System health monitoring
- **LAN_BRIDGE_AGENT_GUIDE.md** - Cross-machine communication

---

## 🤝 Contributing

### Adding a New Agent

**Option 1: Automated (Recommended)**
1. Create markdown spec in `agents/incoming_specs/`
2. BuilderAgent generates agent code automatically
3. Test and refine generated code

**Option 2: Manual**
1. Create `agents/your_agent.py` extending `AgentBase`
2. Implement `run()` and `handle_message()` methods
3. Add to `agents/__init__.py`
4. Update `start_all_agents.bat`
5. Update `versions.json`
6. Create tests in `tests/test_your_agent.py`

### Code Quality Standards
- **Async-first**: Use `async`/`await` for all I/O operations
- **Type hints**: All function signatures must have type annotations
- **Error handling**: Graceful degradation, never crash
- **Logging**: Use structured logging (`logger.info`, `logger.debug`)
- **Testing**: Unit tests + integration tests required

---

## 🔗 Quick Links

- **Dashboard**: http://localhost:8080
- **Manager Server**: ws://localhost:9000
- **Documentation**: `.github/copilot-instructions.md`
- **Issue Tracker**: See `AGENT_QUALITY_CHECK_REPORT.md`

---

**Version**: 3.0 | **Last Updated**: 2025-01-19 | **Status**: Production Ready ✅
- godot-mcp, godot-docs-mcp (game dev)
- postgres-mcp (database)

## 📖 Documentation

### Key Guides
- **BUILDERAGENT_INTEGRATION_COMPLETE.md**: Complete BuilderAgent documentation
- **BUILDERAGENT_TESTING_GUIDE.md**: Testing procedures for BuilderAgent
- **ASYNC_PHASE2_IMPLEMENTATION.md**: Async conversion patterns
- **AUTONOMOUS_AGENTS_GUIDE.md**: Self-healing agent capabilities

### Development Docs
- **.github/copilot-instructions.md**: AI coding agent instructions
- **docs/SESSION_*.md**: Session summaries and progress tracking
- **docs/testing/**: Testing guides for all features

## 🎯 Roadmap

### Phase 3F (Current - October 2025)
- ✅ BuilderAgent integration complete
- 🔄 LocustAgent generation (IN PROGRESS)
- ⏳ Additional specialized agents

### Phase 4 (Q4 2025)
- Enhanced autonomous capabilities
- Multi-machine orchestration
- Advanced analytics dashboard
- CI/CD pipeline integration

**Status**: Development Phase | **Version**: 1.0 | **Last Updated**: October 21, 2025
