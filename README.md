# 🚀 PM OS — Your Personal AI-Powered Product Development System

> A complete lifecycle system for taking a product from raw idea to POC launch,
> powered by Claude agents, Markdown artifacts, and Claude Code in VS Code.

---

## What Is This?

PM OS is a collection of **reusable AI agents** and **structured Markdown templates** that automate the cognitive heavy lifting of product development. Each agent is a Claude system prompt you paste into a Claude.ai Project — turning that Project into a specialized assistant for one phase of your workflow.

Every agent produces a **Markdown artifact** that becomes the input to the next agent. By the time you reach Claude Code in VS Code, you have a full specification ready to build from.

---

## The Full System

```
Your Raw Idea
     │
     ▼
┌─────────────────────────────────────────────┐
│  PHASE 1 — Ideation & Market Intelligence   │
│                                             │
│  01-ideation-agent          → DISCOVERY.md  │
│  01b-competitive-agent      → COMPETITIVE.md│
└─────────────────────────────────────────────┘
     │
     ▼
┌─────────────────────────────────────────────┐
│  PHASE 2 — Requirements & Planning          │
│                                             │
│  02-user-story-agent  → USER_STORIES.md     │
│                         PRD.md              │
│  02b-roadmap-agent    → ROADMAP.md          │
└─────────────────────────────────────────────┘
     │
     ▼
┌─────────────────────────────────────────────┐
│  PHASE 3 — Design Specification             │
│                                             │
│  03-design-spec-agent → DESIGN_SPEC.md      │
└─────────────────────────────────────────────┘
     │
     ▼
┌─────────────────────────────────────────────┐
│  PHASE 4 — Build (Claude Code in VS Code)   │
│                                             │
│  Feed: DESIGN_SPEC.md + USER_STORIES.md     │
│        + PRD.md as context                  │
│  Output: /src POC codebase                  │
└─────────────────────────────────────────────┘
     │
     ▼
┌─────────────────────────────────────────────┐
│  PHASE 5 — Evals & Quality                  │
│                                             │
│  04-eval-agent → EVALS.md + TEST_CASES.md   │
└─────────────────────────────────────────────┘
     │
     ▼
┌─────────────────────────────────────────────┐
│  PHASE 6 — Launch                           │
│                                             │
│  05-launch-agent → METRICS.md + LAUNCH.md  │
└─────────────────────────────────────────────┘
```

---

## File Structure

```
pm-os/
├── README.md                          ← You are here
│
├── agents/                            ← Paste these into Claude.ai Projects
│   ├── 01-ideation-agent.md           ← Phase 1: Raw idea → DISCOVERY.md + pitch
│   ├── 01b-competitive-agent.md       ← Phase 1: Market research → COMPETITIVE.md
│   ├── 02-user-story-agent.md         ← Phase 2: Discovery → User Stories + PRD
│   ├── 02b-roadmap-agent.md           ← Phase 2: Stories → ROADMAP.md
│   ├── 03-design-spec-agent.md        ← Phase 3: Stories → DESIGN_SPEC.md
│   ├── 04-eval-agent.md               ← Phase 5: Stories + code → EVALS.md
│   └── 05-launch-agent.md             ← Phase 6: All artifacts → LAUNCH.md
│
├── templates/                         ← Blank templates for each artifact
│   ├── DISCOVERY.md
│   ├── COMPETITIVE.md
│   ├── PRD.md
│   ├── USER_STORIES.md
│   ├── ROADMAP.md
│   ├── DESIGN_SPEC.md
│   ├── EVALS.md
│   ├── TEST_CASES.md
│   ├── METRICS.md
│   └── LAUNCH.md
│
└── projects/                          ← One folder per product you build
    └── [your-product-name]/
        ├── DISCOVERY.md
        ├── COMPETITIVE.md
        ├── PRD.md
        ├── USER_STORIES.md
        ├── ROADMAP.md
        ├── DESIGN_SPEC.md
        ├── EVALS.md
        └── LAUNCH.md
```

---

## How to Use Each Agent

### Step 1 — Set Up Claude.ai Projects

Create a **separate Claude.ai Project for each agent**. Each Project gets one system prompt.

| Project Name | System Prompt File | What You Feed It |
|---|---|---|
| `PM OS — Ideation` | `01-ideation-agent.md` | Your raw idea (any format) |
| `PM OS — Competitive Research` | `01b-competitive-agent.md` | `DISCOVERY.md` |
| `PM OS — User Stories` | `02-user-story-agent.md` | `DISCOVERY.md` |
| `PM OS — Roadmap` | `02b-roadmap-agent.md` | `USER_STORIES.md` |
| `PM OS — Design Spec` | `03-design-spec-agent.md` | `USER_STORIES.md` + `PRD.md` |
| `PM OS — Evals` | `04-eval-agent.md` | `USER_STORIES.md` + code snippets |
| `PM OS — Launch` | `05-launch-agent.md` | All `.md` artifacts |

**How to create a Project:**
1. Go to claude.ai
2. Click "Projects" in the left sidebar
3. Click "New Project"
4. Paste the contents of the `.md` agent file into the **"Project Instructions"** field
5. Name the project (e.g., "PM OS — Ideation")

---

### Step 2 — Run Phase 1: Ideation

1. Open your `PM OS — Ideation` Project
2. Type or paste your product idea (rough is fine)
3. Answer the agent's clarifying questions
4. The agent will produce `DISCOVERY.md` content + a Pitch Deck brief
5. Copy the `DISCOVERY.md` output → save to `projects/[your-product]/DISCOVERY.md`
6. Use the Pitch Deck brief as input to generate the actual PPTX (or ask Claude to build it)

---

### Step 3 — Run Phase 1b: Competitive Research

1. Open your `PM OS — Competitive Research` Project
2. Paste your `DISCOVERY.md` as context
3. The agent will search for competitors and produce `COMPETITIVE.md`
4. Save to `projects/[your-product]/COMPETITIVE.md`

---

### Step 4 — Run Phase 2: User Stories + PRD

1. Open your `PM OS — User Stories` Project
2. Paste `DISCOVERY.md` + `COMPETITIVE.md` as context
3. The agent will generate Epics → Features → User Stories with acceptance criteria
4. Also produces a structured `PRD.md`
5. Save both to your project folder

---

### Step 5 — Run Phase 2b: Roadmap

1. Open your `PM OS — Roadmap` Project
2. Paste `USER_STORIES.md` as context
3. The agent generates `ROADMAP.md` with quarters, themes, priorities, and dependencies

---

### Step 6 — Run Phase 3: Design Spec

1. Open your `PM OS — Design Spec` Project
2. Paste `USER_STORIES.md` + `PRD.md` as context
3. The agent generates `DESIGN_SPEC.md` with UX flows, component inventory, screen states

---

### Step 7 — Build with Claude Code in VS Code

This is where the Markdown chain becomes working code.

**Setup:**
1. Install the Claude Code extension in VS Code
2. Create a new folder: `projects/[your-product]/src/`
3. Copy all your `.md` artifacts into the project root

**How to prompt Claude Code:**
```
Read the following files for context before writing any code:
- DESIGN_SPEC.md (component inventory + UX flows)
- USER_STORIES.md (features + acceptance criteria)
- PRD.md (technical requirements)

Now scaffold the project structure and implement [specific feature].
```

**Pro tips for Claude Code:**
- Reference specific user story IDs when asking for features (e.g., "Implement US-003")
- Ask Claude Code to write tests based on acceptance criteria in USER_STORIES.md
- Paste the DESIGN_SPEC component list to have Claude scaffold all components at once

---

### Step 8 — Run Phase 5: Evals

1. Open your `PM OS — Evals` Project
2. Paste `USER_STORIES.md` + key code snippets as context
3. The agent generates `EVALS.md` (per-story success criteria) + `TEST_CASES.md`

---

### Step 9 — Run Phase 6: Launch

1. Open your `PM OS — Launch` Project
2. Paste all `.md` artifacts as context
3. The agent generates `METRICS.md` (KPI dashboard spec) + `LAUNCH.md` (Go/No-Go checklist, demo script, stakeholder update)

---

## Quick Reference: Artifact Dependency Map

```
DISCOVERY.md ──────────────┬──→ COMPETITIVE.md
                           ├──→ USER_STORIES.md ──┬──→ DESIGN_SPEC.md ──→ /src code
                           │                      ├──→ ROADMAP.md
                           │                      ├──→ EVALS.md
                           │                      └──→ TEST_CASES.md
                           └──→ PRD.md ────────────→ DESIGN_SPEC.md

All artifacts ─────────────────────────────────────→ LAUNCH.md + METRICS.md
```

---

## Pro Tips

**1. Keep artifacts versioned**
Add `v1.0`, `v1.1` to filenames as they evolve. Never overwrite — always fork.

**2. Use artifacts as conversation starters**
When talking to stakeholders, paste the relevant `.md` file and say: "Here's what the AI produced — let's pressure test it."

**3. The Pitch Deck is always up to date**
Re-run the Ideation Agent after major pivots to regenerate the pitch deck brief. Takes 5 minutes.

**4. Claude Code works best with specific story IDs**
Format: "Implement the acceptance criteria for US-003 (User can filter results by category)"

**5. Treat DISCOVERY.md as the source of truth**
If anything changes fundamentally, update DISCOVERY.md first, then cascade downstream.

---

## Agent Build Status

| Agent | Status | File |
|-------|--------|------|
| Ideation Agent | ✅ Built | `01-ideation-agent.md` |
| Competitive Research Agent | 🔜 Next | `01b-competitive-agent.md` |
| User Story Agent | 🔜 Phase 2 | `02-user-story-agent.md` |
| Roadmap Agent | 🔜 Phase 2 | `02b-roadmap-agent.md` |
| Design Spec Agent | 🔜 Phase 3 | `03-design-spec-agent.md` |
| Eval Agent | 🔜 Phase 5 | `04-eval-agent.md` |
| Launch Agent | 🔜 Phase 6 | `05-launch-agent.md` |

---

*Built with Claude. Designed for PMs who move fast.*
