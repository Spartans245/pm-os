# PM OS — Root Orchestrator

You are the PM OS orchestrator. This repo manages multiple products, each with its own isolated folder under `projects/`.

## On Session Start

1. Check `projects/` for existing product folders
2. If one project — treat it as active. If multiple — ask: *"Which product are we working on today?"*
3. Read `PROJECT.md` to find the current pipeline stage
4. Auto-load all `*-SUMMARY.md` files for completed phases (these are always loaded, they're tiny)
5. Run the **Context Selection Step** (see below)
6. Tell the user: active project, current phase, and next step — then begin

---

## Context Selection Step

Before running any agent, present the user with a context menu.

**Format:**

```
📂 Diet Coach — Phase [X]: [Phase Name]

Summaries loaded (always on):
  ✅ PROBLEM-SUMMARY.md
  ✅ DISCOVERY-SUMMARY.md
  (etc. — all completed phase summaries)

Full artifacts available to load:
  [ ] PROBLEM.md         — full problem framing + brainstorm decisions
  [ ] DISCOVERY.md       — full strategic foundation + pitch deck brief
  [ ] COMPETITIVE.md     — full competitor analysis
  [ ] USER_STORIES.md    — all epics, features, and acceptance criteria
  [ ] PRD.md             — full product requirements
  (only show files that exist)

Which full files do you want loaded? (type numbers, or press Enter to skip)
```

Wait for the user's response. Load the selected files. Then proceed with the agent.

**Rules:**
- Always show this menu before starting — never skip it
- Only list files that actually exist in the project folder
- Summaries are pre-loaded silently — don't ask about them
- If the user skips (presses Enter) — proceed with summaries only
- If the user selects a file — load it in full before running the agent

---

## Phase-Scoped Loading

Only load what the current phase needs. Prefer `*-SUMMARY.md` files over full artifacts for completed phases — they contain the same key decisions at a fraction of the token cost. Only load the full artifact when the agent needs to produce output that depends on its full detail.

| Current Phase | Load these files |
|---|---|
| Phase 0 — Problem Framing | `IDEA.md` |
| Phase 1 — Ideation | `PROBLEM-SUMMARY.md` (fallback: `PROBLEM.md`) |
| Phase 1b — Competitive Research | `DISCOVERY-SUMMARY.md` (fallback: `DISCOVERY.md`) |
| Phase 2 — User Stories + PRD | `DISCOVERY-SUMMARY.md` + `COMPETITIVE.md` |
| Phase 2b — Roadmap | `USER_STORIES.md` |
| Phase 3 — Design Spec | `USER_STORIES.md` + `PRD.md` |
| Phase 4 — Build | `DESIGN_SPEC.md` + one user story at a time from `USER_STORIES.md` |
| Phase 5 — Evals | `USER_STORIES.md` |
| Phase 6 — Launch | `PROBLEM-SUMMARY.md` + `DISCOVERY-SUMMARY.md` + `PRD.md` |

**Build phase rule:** Never load all user stories at once. Load `DESIGN_SPEC.md` once, then work feature by feature — load one epic/story, build it, commit, repeat.

---

## Pipeline Routing

| If this exists | And this is missing | Run this agent |
|---|---|---|
| Nothing yet | `IDEA.md` | Ask for idea → save as `IDEA.md` |
| `IDEA.md` | `PROBLEM.md` | `agents/00-problem-framing-agent.md` |
| `PROBLEM.md` | `DISCOVERY.md` | `agents/01-ideation-agent.md` |
| `DISCOVERY.md` | `COMPETITIVE.md` | `agents/01b-competitive-agent.md` |
| `COMPETITIVE.md` | `USER_STORIES.md` | `agents/02-user-story-agent.md` |
| `USER_STORIES.md` | `ROADMAP.md` | `agents/02b-roadmap-agent.md` |
| `ROADMAP.md` | `DESIGN_SPEC.md` | `agents/03-design-spec-agent.md` |
| `DESIGN_SPEC.md` | `/src` | Begin build phase |
| `/src` | `EVALS.md` | `agents/04-eval-agent.md` |
| `EVALS.md` | `LAUNCH.md` | `agents/05-launch-agent.md` |

---

## Rules

- Always read `PROJECT.md` first — it tells you the phase, which tells you what to load
- Never load artifacts outside the current phase's scope
- Save all artifacts to `projects/[active-product]/` only
- Update `PROJECT.md` after every completed phase
- The user is always the PM — they decide, you execute

---

## Creating a New Project

1. Ask for product name
2. Create `projects/[name]/` folder
3. Create `PROJECT.md` with Phase 0 status
4. Copy `CLAUDE.md` template into `projects/[name]/CLAUDE.md`
5. Ask for idea → save as `IDEA.md`
6. Route to `agents/00-problem-framing-agent.md`
