# QuantCoder_FS – Full-Stack AI Workflow Platform

**Open-Source Research Platform for Quantitative Finance**  
*Version 0.2 – Multi-Workflow AI Agent Platform (Development Branch)*  
*Author: Sebastien M. Laignel*

---

## Table of Contents

1. [Introduction](#introduction)  
2. [Backend Architecture](#backend-architecture)  
3. [Frontend Architecture](#frontend-architecture)  
4. [Implemented Workflows](#implemented-workflows)  
5. [Contributing](#contributing)

---

## Introduction

**QuantCoder_FS** is a full-stack AI platform combining a public **FastAPI + CrewAI** backend and a private **Next.js** frontend. It orchestrates intelligent agents to automate core research tasks for quantitative finance: PDF summarisation, fundamental data parsing, and algorithmic idea generation.

Only the **backend** is open-source. The frontend remains private and handles authentication, interface state, and routing. This repository provides backend workflows as reusable, modular components.

---

## Backend Architecture

### Folder / File Structures

```
backend/
├── main.py                  # FastAPI entry point
├── routers/                 # API endpoints
├── agents/                  # CrewAI agent definitions
├── workflows/               # Agent task orchestration
├── tools/                   # API connectors, utilities
├── models/                  # Pydantic schemas
├── utils/                   # Logging, file utils, LLM costs
├── core/                    # Config and env
├── db/                      # SQLite (extensible)
├── scripts/                 # Setup scripts
├── tests/                   # Unit/integration tests
└── .github/workflows/       # GitHub Actions CI
```

### How the Backend Works

- **Routers** expose RESTful endpoints
- **Workflows** define CrewAI logic
- **Agents** serve research roles and use tools
- **Models** validate typed output
- **Utilities** support file handling, logging, and cost tracking

### Example: Summarisation

```python
@router.post("/extract")
async def extract_summary(file: UploadFile):
    pdf_path = save_uploaded_file(file)
    result = summarization_workflow.kickoff(inputs={"pdf_path": pdf_path}).to_dict()
    return result
```

---

## Frontend Architecture

> The frontend is **private** and not part of this repository. This section documents its structure for understanding integration points.

### Folder Structure

```
frontend/
├── pages/                   # Workflow routes
├── components/              # Reusable and workflow-specific UI components
├── lib/                     # API call helpers
├── types/                   # Shared TypeScript types
├── styles/                  # Tailwind & custom CSS
├── next.config.js
└── .env.local
```

### Key Components

- **SavedSummariesList**: Sidebar for workflow switching
- **SummaryDisplay**, **CodeViewer**, **MetricsTable**: Render workflow output
- **ChatWindow**: Entry point for fundamental analysis
- Zustand is used for global state

### Workflow Integration

- Workflows live in individual routes (`/summarizer`, `/fundamentals`, etc.)
- Each component fetches backend data using `lib/api.ts`
- User transitions between workflows without page reloads

### Architecture Diagram

```mermaid
graph TD
  A[User Interface] -->|Chat, Upload, Select| B[Next.js Frontend]
  B --> C[API Calls via lib/api.ts]
  C --> D[FastAPI Routers]
  D --> E[CrewAI Workflows]
  E --> F[Agents + Tools]
  F --> G[External APIs (EODHD, OpenAI)]
  E --> H[Pydantic Output Models]
  D --> I[Response to Frontend]
  B --> J[State Management (Zustand)]
```

---

## Implemented Workflows

### Summarisation

- Uploads PDF, extracts: methodology, results, future research
- Backend only: `summarymodel.py`, `summarization_agents.py`, `summarization_workflow.py`

### Chat with Fundamentals

- Accepts user queries like "What’s the outlook for AAPL?"
- Retrieves: executive summary, news, metrics, charts
- Pydantic model: `UserQuery`
- Agent chain powered by EODHD API
- Known issues: latency, model compatibility

---

## Contributing

We welcome contributions to the backend workflows.

### Contribution Guidelines

1. **Fork** this repository
2. Create a branch from `dev` with a name like `feat/your-workflow-name`
3. Make your changes in a modular way:
   - Add agents to `agents/`
   - Add workflows to `workflows/`
   - Add models to `models/`
   - Update routers minimally
4. **Submit a Pull Request** to the `dev` branch
   - Describe the purpose and agent logic in the PR
   - Reference any related Issues if applicable

### Issues

Use GitHub Issues to:
- Request new agent workflows
- Report bugs in existing backend logic
- Propose changes to folder structure, cost tracking, etc.

---

_End of documentation._

