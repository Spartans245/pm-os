# PM OS — AI-Powered Product Development System

> A complete lifecycle system for taking a product from raw idea to POC launch,
> powered by Claude agents, Markdown artifacts, and Claude Code in VS Code.

---

## What Is This?

PM OS is a collection of **reusable AI agents** and **structured Markdown templates** that automate the cognitive heavy lifting of product development. Each agent runs in Claude Code and produces a **Markdown artifact** saved directly into the active product's folder. By the time you reach the Build phase, you have a full specification ready to build from.

Each product lives in its own folder under `projects/`. All artifacts for that product are saved there — creating a persistent, compounding context that every downstream agent reads before it runs.

---

## The Full System

```
Your Raw Idea
     │
     ▼
┌─────────────────────────────────────────────┐
│  PHASE 0 — Problem Discovery                │
│                                             │
│  problem-framing-agent   → PROBLEM.md    │
└─────────────────────────────────────────────┘
     │
     ▼
┌─────────────────────────────────────────────┐
│  PHASE 1 — Ideation & Market Intelligence   │
│                                             │
│  ideation-agent          → DISCOVERY.md  │
│  competitive-research-agent      → COMPETITIVE.md│
└─────────────────────────────────────────────┘
     │
     ▼
┌─────────────────────────────────────────────┐
│  PHASE 2 — Requirements & Planning          │
│                                             │
│  user-stories-agent  → USER_STORIES.md     │
│                         PRD.md              │
│  roadmap-agent    → ROADMAP.md          │
└─────────────────────────────────────────────┘
     │
     ▼
┌─────────────────────────────────────────────┐
│  PHASE 3 — Design Specification             │
│                                             │
│  design-spec-agent → DESIGN_SPEC.md      │
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
│  eval-agent → EVALS.md + TEST_CASES.md   │
└─────────────────────────────────────────────┘
     │
     ▼
┌─────────────────────────────────────────────┐
│  PHASE 6 — Launch                           │
│                                             │
│  launch-agent → METRICS.md + LAUNCH.md   │
└─────────────────────────────────────────────┘
```

---

## File Structure

```
pm-os/
├── README.md                          ← You are here
│
├── skills/                            ← Agent instructions (the OS brain)
│   ├── problem-framing.md    ← Phase 0: Raw idea → PROBLEM.md
│   ├── ideation.md           ← Phase 1: Problem → DISCOVERY.md + pitch
│   ├── competitive-research.md       ← Phase 1: Market research → COMPETITIVE.md
│   ├── user-stories.md         ← Phase 2: Discovery → User Stories + PRD
│   ├── roadmap.md           ← Phase 2: Stories → ROADMAP.md
│   ├── design-spec.md        ← Phase 3: Stories → DESIGN_SPEC.md
│   ├── eval.md               ← Phase 5: Stories + code → EVALS.md
│   └── launch.md             ← Phase 6: All artifacts → LAUNCH.md
│
├── templates/                         ← Blank templates for each artifact
│   ├── PROBLEM.md
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
└── projects/                          ← One folder per product (persistent context)
    └── [your-product-name]/
        ├── PROJECT.md                 ← Pipeline status tracker
        ├── IDEA.md                    ← Seed: your raw input, saved first
        ├── PROBLEM.md                 ← Phase 0 output
        ├── DISCOVERY.md               ← Phase 1 output
        ├── COMPETITIVE.md
        ├── PRD.md
        ├── USER_STORIES.md
        ├── ROADMAP.md
        ├── DESIGN_SPEC.md
        ├── EVALS.md
        └── LAUNCH.md
```

---

## How to Use the OS

### Step 1 — Create a New Product Folder

```
projects/
  your-product-name/
    PROJECT.md    ← create this first to track pipeline status
```

Share your raw idea — it gets saved as `IDEA.md` in the product folder. This is the seed that feeds every downstream agent.

---

### Step 2 — Run Phase 0: Problem Framing

Point Claude Code at `skills/problem-framing.md`. The agent will:
1. Read your `IDEA.md`
2. Ask at most 2 clarifying questions
3. Brainstorm with you and offer 3 reframes
4. Write `PROBLEM.md` once you sign off

Save output to `projects/[your-product]/PROBLEM.md`.

---

### Step 3 — Run Phase 1: Ideation

Reads: `PROBLEM.md`
Produces: `DISCOVERY.md` + Pitch Deck brief

---

### Step 4 — Run Phase 1b: Competitive Research

Reads: `DISCOVERY.md`
Produces: `COMPETITIVE.md`

---

### Step 5 — Run Phase 2: User Stories + PRD

Reads: `DISCOVERY.md` + `COMPETITIVE.md`
Produces: `USER_STORIES.md` + `PRD.md`

---

### Step 6 — Run Phase 2b: Roadmap

Reads: `USER_STORIES.md`
Produces: `ROADMAP.md`

---

### Step 7 — Run Phase 3: Design Spec

Reads: `USER_STORIES.md` + `PRD.md`
Produces: `DESIGN_SPEC.md`

---

### Step 8 — Build with Claude Code in VS Code

Reads: `DESIGN_SPEC.md` + `USER_STORIES.md` + `PRD.md`
Produces: `/src` POC codebase

Claude Code reads all artifacts in the project folder automatically. Reference specific user story IDs when building features (e.g., "Implement US-003").

---

### Step 9 — Run Phase 5: Evals

Reads: `USER_STORIES.md` + code
Produces: `EVALS.md` + `TEST_CASES.md`

---

### Step 10 — Run Phase 6: Launch

Reads: All artifacts
Produces: `METRICS.md` + `LAUNCH.md`

---

## Artifact Dependency Map

```
IDEA.md ────────────────────→ PROBLEM.md
                                   │
                                   ▼
                             DISCOVERY.md ──┬──→ COMPETITIVE.md
                                            │
                                            ├──→ USER_STORIES.md ──┬──→ DESIGN_SPEC.md ──→ /src
                                            │                      ├──→ ROADMAP.md
                                            │                      ├──→ EVALS.md
                                            │                      └──→ TEST_CASES.md
                                            └──→ PRD.md ───────────→ DESIGN_SPEC.md

All artifacts ──────────────────────────────────────────────────────→ LAUNCH.md + METRICS.md
```

---

## Skill Build Status

| Phase | Agent | Status | File |
|-------|-------|--------|------|
| 0 | Problem Framing Agent | ✅ Built | `problem-framing.md` |
| 1 | Ideation Agent | ✅ Built | `ideation.md` |
| 1b | Competitive Research Agent | 🔜 Pending | `competitive-research.md` |
| 2 | User Story Agent | 🔜 Pending | `user-stories.md` |
| 2b | Roadmap Agent | 🔜 Pending | `roadmap.md` |
| 3 | Design Spec Agent | 🔜 Pending | `design-spec.md` |
| 5 | Eval Agent | 🔜 Pending | `eval.md` |
| 6 | Launch Agent | 🔜 Pending | `launch.md` |

---

*Built with Claude. Designed for PMs who move fast.*
