# GitHub API Surface — Thinking for Agent-Scale Consumption

*By Eric Armstrong | Staff PM Application*

---

## Starting Point: What the Current API Was Built For

GitHub's REST and GraphQL APIs are exceptional at what they were designed to do — give developers and applications programmatic access to repositories, pull requests, issues, actions, and the full GitHub object model.

The design assumptions baked into that surface make sense for human-paced, app-mediated interaction:

- **Authentication** is scoped to users and OAuth apps
- **Rate limits** are per-token, shared across all callers using that token
- **Operations** are atomic and single-purpose
- **State** lives in the repository — callers are expected to reconstruct context from that state on each interaction
- **Webhooks** push events to registered endpoints when things change

These assumptions worked well for the last 15 years. They are the wrong foundation for agent-scale consumption.

---

## The Four API Problems Agents Surface

### Problem 1: Rate Limits Are the Wrong Unit

Current rate limits are token-scoped and operation-counted. An agent doing meaningful work — reading a codebase, proposing changes, validating output, iterating — might require thousands of API calls for a single logical task.

Today that agent competes for rate limit budget with every other tool using the same token. There's no way to say "this is a bounded agent task with a start and an end — evaluate my usage against the task, not against a rolling window."

**What I'd propose:** Agent task sessions with dedicated rate limit budgets scoped to the task duration. An agent declares "I am starting a task" with an estimated scope, receives a session token with a corresponding budget, and that budget is isolated from the developer's other tooling usage.

### Problem 2: No Native Batch Operations for Read-Evaluate-Write Cycles

Agents frequently need to: read multiple files, evaluate their contents, and write changes — as a single logical operation. Today that's 3–10+ individual API calls with no transactional guarantee. If the agent is interrupted mid-cycle, the repository can be left in a partial state with no clean rollback path.

**What I'd propose:** Compound operation endpoints for common agent patterns. Not a generic transaction system — that's too complex to ship fast — but purpose-built batch endpoints for the 5–6 most common agent workflows: read-and-propose, evaluate-and-comment, refactor-and-PR, etc.

### Problem 3: Context Has to Be Reconstructed Every Session

When an agent starts work on a repository, it has no memory of previous sessions unless it builds and stores that memory itself. GitHub has all the information needed to provide rich session context — commit history, PR history, issue links, code ownership, recent changes — but there's no endpoint that assembles that context for an agent starting a new task.

**What I'd propose:** A repository context endpoint designed for agent consumption. Not a dump of all repository data — a structured, configurable summary: recent activity, open work items, ownership map, dependency graph snapshot, and last-known agent session state. One call, meaningful context.

### Problem 4: Write Operations Have No Intent Layer

When a developer opens a PR, the title and description carry intent. When an agent opens a PR, the description is often either empty, auto-generated boilerplate, or a change log — not a statement of intent that a developer can evaluate and trust.

This isn't just a UX problem. It's a trust problem. Developers don't merge PRs they don't understand. If agents are going to become productive collaborators on GitHub, their write operations need to carry structured intent that GitHub's UI can surface clearly.

**What I'd propose:** Optional structured intent fields on write operations — PR opens, commit pushes, issue comments. Standardized schema: `goal`, `approach`, `confidence`, `risks`, `human_review_recommended`. Agent-authored, surfaced natively in GitHub's review UI.

---

## What I Would Not Do

It would be tempting to design a completely separate "agent API" — a parallel surface with different auth, different endpoints, different conventions. I'd argue against that for two reasons:

1. **Ecosystem fragmentation.** Every tool that has to choose between "developer API" and "agent API" is a tool that might build for one and not the other. GitHub's strength is its unified ecosystem. The agent surface should extend the existing API, not fork it.

2. **The line between developer and agent is blurring.** Copilot in the editor is already an agent making API calls on a developer's behalf. The distinction between "a developer using GitHub" and "an agent using GitHub for a developer" will continue to collapse. The API needs to handle both gracefully, not bifurcate them.

The right architecture is additive: agent-native extensions on top of the existing surface, with backwards compatibility as a hard constraint.

---

## How I'd Sequence This

**Phase 1 — Foundation (0–6 months)**
Agent identity and session tokens. This is the prerequisite for everything else. Nothing else in the agent platform story works without a clean answer to "who is this agent and what is it allowed to do."

**Phase 2 — Context (6–12 months)**
Repository context endpoint and agent session state. Dramatically reduce the cold-start problem for agents beginning new tasks.

**Phase 3 — Operations (12–18 months)**
Compound operation endpoints for the most common agent workflows. Start with the patterns already being used by Copilot, Cursor, and Devin — standardize what the ecosystem is already building workarounds for.

**Phase 4 — Intent (18–24 months)**
Structured intent fields on write operations. This is last not because it's least important — it may be the most important for enterprise trust — but because it requires UI investment across GitHub's review surfaces, not just API changes.

---

## The Principle Underneath All of This

API design for agents is product design for trust.

Every decision on rate limits, batch operations, context, and intent comes back to the same question: does a developer watching an agent work on their codebase understand what's happening and feel in control?

If yes, adoption follows. If no, nothing else matters.

That's the lens I'd bring to every API roadmap decision on this team.
