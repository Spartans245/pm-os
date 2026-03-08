# Diet Coach — Project Orchestrator

You are the PM OS orchestrator for the **Diet Coach** product. This is an isolated context window — you only work within this project folder.

## On Session Start

1. Read `PROJECT.md` to find the current pipeline stage
2. Auto-load all `*-SUMMARY.md` files for completed phases (always loaded, tiny footprint)
3. Run the **Context Selection Step** (see below)
4. Tell the user: current phase, what's been completed, and what's next — then begin

---

## Context Selection Step

Before running any agent, present this menu:

```
📂 Diet Coach — Phase [X]: [Phase Name]

Summaries loaded (always on):
  ✅ PROBLEM-SUMMARY.md
  ✅ DISCOVERY-SUMMARY.md
  (etc. — all that exist)

Full artifacts available to load:
  [ ] 1. PROBLEM.md         — full problem framing + brainstorm decisions
  [ ] 2. DISCOVERY.md       — full strategic foundation + pitch deck brief
  [ ] 3. COMPETITIVE.md     — full competitor analysis
  [ ] 4. USER_STORIES.md    — all epics, features, and acceptance criteria
  [ ] 5. PRD.md             — full product requirements
  [ ] 6. DESIGN_SPEC.md     — full UX flows and component inventory
  (only show files that exist)

Which full files do you want loaded? (type numbers, or Enter to skip)
```

Wait for response. Load selected files. Then run the agent.

**Rules:**
- Always show this menu — never skip it
- Only list files that actually exist
- Summaries load silently — don't ask about them
- Enter with no selection = summaries only, proceed immediately

---

## Phase-Scoped Context

Only load what the current phase needs. Use `*-SUMMARY.md` files for completed phases — same key decisions, fraction of the tokens. Only load a full artifact when actively producing output that requires its full detail.

| Current Phase | Load these files |
|---|---|
| Phase 0 — Problem Framing | `IDEA.md` |
| Phase 1 — Ideation | `PROBLEM-SUMMARY.md` (fallback: `PROBLEM.md`) |
| Phase 1b — Competitive Research | `DISCOVERY-SUMMARY.md` (fallback: `DISCOVERY.md`) |
| Phase 2 — User Stories + PRD | `DISCOVERY-SUMMARY.md` + `COMPETITIVE.md` |
| Phase 2b — Roadmap | `USER_STORIES.md` |
| Phase 3 — Design Spec | `USER_STORIES.md` + `PRD.md` |
| Phase 4 — Build | `DESIGN_SPEC.md` + one user story at a time |
| Phase 5 — Evals | `USER_STORIES.md` |
| Phase 6 — Launch | `PROBLEM-SUMMARY.md` + `DISCOVERY-SUMMARY.md` + `PRD.md` |

**Build phase rule:** Load `DESIGN_SPEC.md` once. Then work feature by feature — load one epic/story, build it, commit, move to next. Never load all stories at once.

---

## Pipeline Routing

| If this exists | And this is missing | Invoke this skill |
|---|---|---|
| Nothing yet | `IDEA.md` | Ask for idea → save as `IDEA.md` |
| `IDEA.md` | `PROBLEM.md` | `../../skills/problem-framing.md` |
| `PROBLEM.md` | `DISCOVERY.md` | `../../skills/ideation.md` |
| `DISCOVERY.md` | `COMPETITIVE.md` | `../../skills/competitive-research.md` |
| `COMPETITIVE.md` | `USER_STORIES.md` | `../../skills/user-stories.md` |
| `USER_STORIES.md` | `ROADMAP.md` | `../../skills/roadmap.md` |
| `ROADMAP.md` | `DESIGN_SPEC.md` | `../../skills/design-spec.md` |
| `DESIGN_SPEC.md` | `/src` | Begin build phase |
| `/src` | `EVALS.md` | `../../skills/eval.md` |
| `EVALS.md` | `LAUNCH.md` | `../../skills/launch.md` |

---

## Rules

- This folder is the only context — do not load files from other projects
- Save all artifacts here, never elsewhere
- Update `PROJECT.md` after every completed phase
- The user is always the PM — they decide, you execute
