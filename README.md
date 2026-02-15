<p align="center">
  <img src="https://img.shields.io/badge/MYTH--2.0-The%20Smith%20of%20Dev-blueviolet?style=for-the-badge&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIyNCIgaGVpZ2h0PSIyNCIgdmlld0JveD0iMCAwIDI0IDI0IiBmaWxsPSJub25lIiBzdHJva2U9IndoaXRlIiBzdHJva2Utd2lkdGg9IjIiIHN0cm9rZS1saW5lY2FwPSJyb3VuZCIgc3Ryb2tlLWxpbmVqb2luPSJyb3VuZCI+PHBvbHlnb24gcG9pbnRzPSIxMyAyIDMgMTQgMTIgMTQgMTEgMjIgMjEgMTAgMTIgMTAgMTMgMiI+PC9wb2x5Z29uPjwvc3ZnPg==" alt="MYTH-2.0" />
</p>

<h1 align="center">⚡ MYTH-2.0 — The Smith of Dev</h1>

<p align="center">
  <strong>An AI-orchestrated development platform that forges applications at the speed of thought.</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Next.js-15-black?style=flat-square&logo=next.js" />
  <img src="https://img.shields.io/badge/TypeScript-5.9-3178C6?style=flat-square&logo=typescript&logoColor=white" />
  <img src="https://img.shields.io/badge/Tailwind_CSS-4-06B6D4?style=flat-square&logo=tailwindcss&logoColor=white" />
  <img src="https://img.shields.io/badge/MongoDB-Atlas-47A248?style=flat-square&logo=mongodb&logoColor=white" />
  <img src="https://img.shields.io/badge/E2B-Sandbox-FF6B35?style=flat-square" />
  <img src="https://img.shields.io/badge/License-Proprietary-red?style=flat-square" />
</p>

<p align="center">
  <a href="#-features">Features</a> •
  <a href="#-architecture">Architecture</a> •
  <a href="#-tech-stack">Tech Stack</a> •
  <a href="#-getting-started">Getting Started</a> •
  <a href="#-modules">Modules</a> •
  <a href="#-api-reference">API Reference</a> •
  <a href="#-testing">Testing</a> •
  <a href="#-contributing">Contributing</a>
</p>

---

## 🧬 What is MYTH-2.0?

**MYTH-2.0** is a comprehensive AI-orchestrated development platform that empowers developers to build, deploy, and manage applications through **six specialized AI modules** — all from a unified interface. Whether you're wiring up autonomous AI agents, spinning up full-stack MERN apps, generating mobile apps, visualizing data, building React apps from prompts, or cloning and transforming websites — MYTH-2.0 does it all, securely sandboxed and powered by multi-model AI.

---

## ✨ Features

| Module | Description |
|---|---|
| **🤖 Agent AI** | Visual node-based canvas (React Flow) for building autonomous AI agent workflows via drag-and-drop |
| **🏗️ MERN Orchestrator** | Generate and deploy full-stack MERN applications with multi-sandbox provisioning |
| **📱 Application AI** | Generate React Native Expo mobile apps with live preview and hot-reload |
| **📊 Data Insight Dashboard** | Python sandbox with Streamlit for data visualization and analysis |
| **💬 Prompt AI** | Generate complete React/TypeScript applications from natural language prompts in < 30 seconds |
| **🌐 URL AI** | Clone, transform, and export existing websites with structural integrity preservation |

### Cross-Cutting Capabilities

- 🧠 **Multi-Model AI** — Choose between GPT-4o, Claude 3.5 Sonnet, Gemini 2.5 Flash, and Llama 3.3 with automatic fallback
- 🔐 **Sandbox Isolation** — All code execution runs in secure E2B sandboxes with enforced CPU, memory, and disk limits
- ⚡ **Real-Time Collaboration** — WebSocket-powered live state synchronization with conflict resolution
- 🔑 **Auth & RBAC** — JWT-based authentication with role-based access control (admin, developer, viewer)
- 💾 **Dual Database** — MongoDB Atlas for document storage + Turso (LibSQL) for edge-compatible SQL
- 🔄 **Hot Reload** — Sub-2-second hot-reload across MERN, mobile, and prompt-generated apps

---

## 🏛️ Architecture

```
┌──────────────────────────────────────────────────────────────────┐
│                      Next.js 15 Frontend                         │
│   (TypeScript 5.9 · Tailwind CSS 4 · Framer Motion · Radix UI)   │
└──────────────────────────────────────────────────────────────────┘
                                │
                                ▼
┌──────────────────────────────────────────────────────────────────┐
│                     API Layer (Next.js Routes)                    │
│  ┌───────────┬─────────────┬──────────────┬───────────────────┐  │
│  │  Agent AI │ MERN Orch.  │ Application  │ Data · Prompt · URL│  │
│  └───────────┴─────────────┴──────────────┴───────────────────┘  │
└──────────────────────────────────────────────────────────────────┘
                                │
          ┌─────────────────────┼─────────────────────┐
          ▼                     ▼                     ▼
┌──────────────────┐  ┌──────────────────┐  ┌──────────────────┐
│  E2B Sandboxes   │  │  AI Model APIs   │  │  Data Layer      │
│  (Code Exec)     │  │  (Multi-Model)   │  │  (Mongo / Turso) │
└──────────────────┘  └──────────────────┘  └──────────────────┘
```

Each module follows a consistent internal pattern:

```
Module
├── Frontend Component (React)
├── API Routes (Next.js App Router)
├── Service Layer (Business Logic)
├── Sandbox Manager (E2B Integration)
├── Database Models (MongoDB)
└── AI Integration (Model Selection & Routing)
```

### Design Principles

| Principle | Description |
|---|---|
| **Modularity** | Each module is independently deployable and testable |
| **Security** | All code execution happens in isolated sandboxes with resource limits |
| **Scalability** | Multi-sandbox provisioning supports concurrent user operations |
| **Resilience** | Automatic fallback between AI models and graceful error handling |
| **Real-time** | Live preview and hot-reload capabilities for rapid iteration |

---

## 🛠️ Tech Stack

### Core

| Technology | Version | Purpose |
|---|---|---|
| [Next.js](https://nextjs.org/) | 15 | Full-stack React framework (App Router) |
| [TypeScript](https://www.typescriptlang.org/) | 5.9 | Type-safe development |
| [Tailwind CSS](https://tailwindcss.com/) | 4 | Utility-first styling |
| [Framer Motion](https://www.framer.com/motion/) | Latest | Animations and transitions |
| [Radix UI](https://www.radix-ui.com/) | Latest | Accessible headless components |

### AI & Execution

| Technology | Purpose |
|---|---|
| [E2B Code Interpreter SDK](https://e2b.dev/) | Secure sandboxed code execution |
| [OpenAI GPT-4o](https://openai.com/) | Primary AI model |
| [Anthropic Claude 3.5 Sonnet](https://anthropic.com/) | Alternative AI model |
| [Google Gemini 2.5 Flash](https://deepmind.google/) | Fast AI model |
| [Meta Llama 3.3](https://llama.meta.com/) | Open-source AI model |

### Data & Infrastructure

| Technology | Purpose |
|---|---|
| [MongoDB Atlas](https://www.mongodb.com/atlas) | Cloud document database |
| [Turso](https://turso.tech/) | Edge-compatible LibSQL database |
| [React Flow](https://reactflow.dev/) | Node-based visual interfaces |
| [Three.js](https://threejs.org/) | 3D bot visualization |

---

## 🚀 Getting Started

### Prerequisites

- **Node.js** ≥ 20.x
- **npm** or **pnpm**
- **MongoDB Atlas** account
- **E2B** API key
- At least one AI model API key (OpenAI, Anthropic, Google, or Meta)

### Installation

```bash
# Clone the repository
git clone https://github.com/AIforBharat/myth-2.0.git
cd myth-2.0

# Install dependencies
npm install

# Copy environment variables
cp .env.example .env.local
```

### Environment Variables

Create a `.env.local` file with the following keys:

```env
# ── Database ──
MONGODB_URI=mongodb+srv://<user>:<password>@<cluster>.mongodb.net/<db>
TURSO_DATABASE_URL=libsql://<database>-<org>.turso.io
TURSO_AUTH_TOKEN=<token>

# ── AI Models ──
OPENAI_API_KEY=sk-...
ANTHROPIC_API_KEY=sk-ant-...
GOOGLE_AI_API_KEY=AI...
META_LLAMA_API_KEY=<key>

# ── Sandbox ──
E2B_API_KEY=e2b_...

# ── Auth ──
JWT_SECRET=<your-secret>
NEXTAUTH_URL=http://localhost:3000
NEXTAUTH_SECRET=<your-secret>
```

### Development Server

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) to access the platform.

### Production Build

```bash
npm run build
npm start
```

---

## 📦 Modules

### 🤖 Agent AI

Build autonomous AI agent workflows using a **visual node-based canvas** powered by React Flow.

- **Drag & drop** node creation with five node types: `input`, `output`, `processor`, `ai_model`, `conditional`
- **Connection validation** with type-safe data flow paths
- **Multi-model support** — select GPT-4o, Claude, Gemini, or Llama per node
- **Persistent storage** — save/load agent graphs to MongoDB with versioning
- **Execution engine** — topological sort-based execution with data passing between nodes
- **Error propagation** — automatic halting of dependent nodes on failure

---

### 🏗️ MERN Orchestrator

Generate and deploy **full-stack MERN applications** with isolated sandbox environments.

- **Code generation** — Express.js backend + React frontend from specifications
- **Multi-sandbox provisioning** — each app runs in its own isolated E2B sandbox
- **MongoDB Atlas integration** — validated database connections with credential management
- **Unique endpoints** — each deployment gets a unique, non-shared URL
- **Hot reload** — code changes reflected within **2 seconds**
- **Resource enforcement** — graceful termination when CPU/memory/disk limits are exceeded

---

### 📱 Application AI (Mobile)

Generate **React Native Expo** mobile applications with real-time preview.

- **Expo-compatible code generation** — screens, navigation, and components
- **Live preview** — real-time rendering in sandbox with preview URL
- **Sub-second hot-reload** — preview updates within **1 second** of component changes
- **Component library** — Radix UI + Tailwind CSS styled components
- **Error detection** — syntax error highlighting with correction suggestions
- **Export ready** — generates deployment-ready Expo project structure

---

### 📊 Data Insight Dashboard

Create data visualizations using **Python in a secure sandbox** with Streamlit.

- **Python sandbox** — Streamlit runtime with package management
- **Code execution** — run Python code with output capture and error tracebacks
- **Dataset management** — upload datasets and access them via standard file paths
- **Fast iteration** — re-execution and dashboard update within **3 seconds**
- **Export** — generate standalone Streamlit applications with all dependencies

---

### 💬 Prompt AI

Generate complete **React/TypeScript applications** from natural language prompts.

- **NLP-powered generation** — parse prompts to extract features, styling preferences, and framework choices
- **Full app scaffold** — components, pages, types, styles, and config files
- **Code validation** — TypeScript type-checking, syntax validation, and linting
- **Live preview** — sandbox rendering with hot-reload
- **Modification preservation** — user edits are preserved across regenerations
- **Performance target** — complete generation within **30 seconds**
- **Auto-fallback** — retries with a different AI model on generation failure

---

### 🌐 URL AI

Clone and transform **existing websites** while preserving structure and styling.

- **Website fetching** — parse HTML, CSS, and JavaScript from any URL
- **Layout preservation** — visual fidelity cloning with styling intact
- **Transformation engine** — apply DOM modifications while maintaining structural integrity
- **Live preview** — real-time preview updates during transformation
- **Dependency bundling** — external dependencies are bundled with fallback implementations
- **Export** — generate standalone HTML/CSS/JS projects

---

## 📡 API Reference

All API endpoints follow RESTful conventions with JSON request/response bodies. Authentication is required via JWT Bearer tokens.

<details>
<summary><strong>🤖 Agent AI Endpoints</strong></summary>

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/agents` | Create a new agent |
| `GET` | `/api/agents/:id` | Retrieve agent configuration |
| `PUT` | `/api/agents/:id` | Update agent configuration |
| `DELETE` | `/api/agents/:id` | Delete an agent |
| `POST` | `/api/agents/:id/execute` | Execute an agent |
| `GET` | `/api/agents/:id/execution/:executionId` | Get execution status |

</details>

<details>
<summary><strong>🏗️ MERN Orchestrator Endpoints</strong></summary>

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/mern/applications` | Create new MERN application |
| `GET` | `/api/mern/applications/:id` | Get application details |
| `PUT` | `/api/mern/applications/:id` | Update application |
| `DELETE` | `/api/mern/applications/:id` | Delete application |
| `POST` | `/api/mern/applications/:id/deploy` | Deploy application |
| `GET` | `/api/mern/applications/:id/logs` | Get sandbox logs |
| `POST` | `/api/mern/applications/:id/code` | Update application code |

</details>

<details>
<summary><strong>📱 Application AI Endpoints</strong></summary>

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/mobile/applications` | Create new mobile app |
| `GET` | `/api/mobile/applications/:id` | Get app details |
| `PUT` | `/api/mobile/applications/:id` | Update app |
| `POST` | `/api/mobile/applications/:id/preview` | Start live preview |
| `GET` | `/api/mobile/applications/:id/preview` | Get preview URL |
| `POST` | `/api/mobile/applications/:id/export` | Export project |

</details>

<details>
<summary><strong>📊 Data Insight Dashboard Endpoints</strong></summary>

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/dashboards` | Create new dashboard |
| `GET` | `/api/dashboards/:id` | Get dashboard |
| `PUT` | `/api/dashboards/:id` | Update dashboard code |
| `POST` | `/api/dashboards/:id/execute` | Execute Python code |
| `POST` | `/api/dashboards/:id/datasets` | Upload dataset |
| `GET` | `/api/dashboards/:id/output` | Get rendered output |
| `POST` | `/api/dashboards/:id/export` | Export dashboard |

</details>

<details>
<summary><strong>💬 Prompt AI Endpoints</strong></summary>

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/prompt-ai/generate` | Generate app from prompt |
| `GET` | `/api/prompt-ai/applications/:id` | Get generated app |
| `PUT` | `/api/prompt-ai/applications/:id` | Update generated app |
| `POST` | `/api/prompt-ai/applications/:id/preview` | Start preview |
| `GET` | `/api/prompt-ai/applications/:id/preview` | Get preview URL |
| `POST` | `/api/prompt-ai/applications/:id/export` | Export project |
| `GET` | `/api/prompt-ai/applications/:id/generation-status` | Get generation status |

</details>

<details>
<summary><strong>🌐 URL AI Endpoints</strong></summary>

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/url-ai/clone` | Clone website from URL |
| `GET` | `/api/url-ai/clones/:id` | Get cloned website |
| `POST` | `/api/url-ai/clones/:id/transform` | Apply transformations |
| `GET` | `/api/url-ai/clones/:id/preview` | Get preview |
| `POST` | `/api/url-ai/clones/:id/export` | Export project |
| `GET` | `/api/url-ai/clones/:id/status` | Get cloning status |

</details>

### Error Response Format

All API errors follow a consistent format:

```json
{
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Invalid input: 'name' field is required",
    "details": { "field": "name" },
    "timestamp": "2026-02-15T11:00:00.000Z",
    "requestId": "req_abc123"
  }
}
```

| Status Code | Usage |
|---|---|
| `200` | Successful operation |
| `201` | Resource created |
| `400` | Validation error |
| `401` | Authentication failure |
| `403` | Authorization failure |
| `404` | Resource not found |
| `500` | Internal server error |
| `503` | AI model unavailable |

---

## 🧪 Testing

MYTH-2.0 uses a dual testing strategy:

### Unit Tests

Test specific examples, edge cases, and error conditions:

```bash
npm run test
```

### Property-Based Tests

Verify universal correctness properties across generated inputs (100+ iterations each):

```bash
npm run test:properties
```

Property tests cover **82 properties** across all modules:

| Module | Properties | Coverage |
|---|---|---|
| Agent AI | 1–6 | Node rendering, connections, execution, error handling |
| MERN Orchestrator | 7–13 | Sandbox provisioning, isolation, hot-reload, resources |
| Application AI | 14–19 | Code generation, preview, export, error detection |
| Data Insight | 20–25 | Sandbox setup, execution, datasets, export |
| Prompt AI | 26–32 | Generation, validation, performance, fallback |
| URL AI | 33–39 | Fetching, cloning, transformation, dependencies |
| AI Models | 40–44 | Model routing, fallback, context preservation |
| Sandbox Security | 45–50 | Execution, isolation, resource limits, cleanup |
| Data Persistence | 51–56 | CRUD, performance, connections, credentials |
| Real-Time | 57–61 | State sync, conflict resolution, sessions |
| Error Handling | 62–66 | User-friendly errors, logging, timeouts |
| Performance | 67–72 | Load times, generation speed, pagination |
| Auth | 73–77 | Login, authorization, sessions, logging |
| API | 78–82 | Validation, rate limiting, status codes |

---

## 📁 Project Structure

```
myth-2.0/
├── app/                          # Next.js 15 App Router
│   ├── api/                      # API Routes
│   │   ├── agents/               # Agent AI endpoints
│   │   ├── mern/                 # MERN Orchestrator endpoints
│   │   ├── mobile/               # Application AI endpoints
│   │   ├── dashboards/           # Data Insight endpoints
│   │   ├── prompt-ai/            # Prompt AI endpoints
│   │   └── url-ai/               # URL AI endpoints
│   ├── dashboard/                # Dashboard pages
│   └── layout.tsx                # Root layout
├── components/                   # Shared React components
├── lib/                          # Shared utilities
│   ├── ai/                       # AI model router & fallback
│   ├── sandbox/                  # E2B sandbox manager
│   ├── db/                       # Database connections
│   └── auth/                     # Authentication & RBAC
├── models/                       # MongoDB data models
├── services/                     # Business logic layer
├── types/                        # TypeScript type definitions
├── tests/                        # Test suites
│   ├── unit/                     # Unit tests
│   └── properties/               # Property-based tests
├── .env.local                    # Environment variables (git-ignored)
├── next.config.ts                # Next.js configuration
├── tailwind.config.ts            # Tailwind CSS configuration
├── tsconfig.json                 # TypeScript configuration
└── package.json                  # Dependencies and scripts
```

---

## 🤝 Contributing

We welcome contributions! Please follow these steps:

1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/amazing-feature`)
3. **Commit** your changes (`git commit -m 'feat: add amazing feature'`)
4. **Push** to the branch (`git push origin feature/amazing-feature`)
5. **Open** a Pull Request

### Code Standards

- TypeScript strict mode enabled
- ESLint + Prettier for code formatting
- Conventional commits for commit messages
- Property tests for all new features
- Unit tests for edge cases and error conditions

---

## 📄 License

This project is proprietary software. All rights reserved.

---

<p align="center">
  <strong>Built with ⚡ by AIforBharat</strong><br/>
  <sub>MYTH-2.0 — Forging the future of development, one module at a time.</sub>
</p>
