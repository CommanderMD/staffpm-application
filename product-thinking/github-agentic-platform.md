# GitHub's Agentic Platform — A Product Perspective

*By Eric Armstrong | Staff PM Application*

---

## The Problem Worth Solving

GitHub is at an inflection point that most platform companies only get once.

The transition from *developer platform* to *agentic development platform* is not an incremental feature release. It is a foundational shift in who — and what — is consuming the platform. The decisions made in the next 18–24 months will determine whether GitHub is the substrate that the next generation of software development runs on, or whether it becomes a legacy interface that agents work around.

This document is my thinking on the product decisions that matter most.

---

## What Agents Need That Developers Don't

When I built OMEGA v2 — a live multi-exchange trading platform with an autonomous agent runtime — I ran into every friction point that agents encounter when interacting with external APIs and platforms. Here's what I learned:

### 1. Context Persistence
A developer opens a PR, reads the diff, understands the codebase history, and makes a decision. An agent has to reconstruct that context from API calls on every session. GitHub's API currently has no native concept of *agent session context* — no way for an agent to say "I am continuing work on this goal from my last session." Every interaction starts cold.

**Product opportunity:** Agent session objects — lightweight, scoped context anchors that let agents resume intent across API calls without re-fetching the entire repository state.

### 2. Semantic Intent Signaling
Current GitHub API calls are purely mechanical: create a branch, open a PR, add a comment. Agents need to signal *why* they're taking an action, not just *what* action they're taking. This matters for auditability, for conflict resolution when multiple agents are working in parallel, and for giving developers meaningful observability into what their agents are doing.

**Product opportunity:** Optional intent metadata on write operations — structured fields that let agents annotate their actions with goal context that surfaces in GitHub's UI and activity feeds.

### 3. Batched and Transactional Operations
Agents operate at a velocity that makes current rate limits and single-operation API patterns a bottleneck. A developer might make 20 API calls in an hour. An agent working on a complex task might need 2,000. The current architecture wasn't designed for this.

**Product opportunity:** Agent-optimized batch endpoints that allow compound operations (read-evaluate-write cycles) to execute as atomic units, with rate limits that are scoped to agent identity rather than shared with human developer sessions.

---

## The Three Roadmap Bets I'd Prioritize

### Bet 1: Agent Identity and Trust Layer
**What:** A first-class identity system for agents — separate from OAuth apps and PATs — with ephemeral credentials, fine-grained operation-level scoping, and a full audit trail per agent per goal.

**Why now:** Enterprise customers are already deploying agents against GitHub at scale. They have no good answer for "which agent did this and why." This is a trust and compliance gap that will drive enterprise churn if not addressed.

**How I'd measure it:** Agent identity adoption rate among GitHub Enterprise customers; reduction in security incidents tied to overly-permissioned tokens; NPS among enterprise security teams.

### Bet 2: Developer Observability for Agentic Workflows
**What:** Native GitHub UI surfaces that let developers watch, understand, and control what their agents are doing — activity timelines, decision logs, one-click rollback, and agent-authored PR descriptions that explain reasoning, not just changes.

**Why now:** The #1 barrier to enterprise adoption of agentic development is trust. Developers don't adopt tools they can't see into. Observability is the trust product.

**How I'd measure it:** Agent-authored PR merge rate; time-to-review on agent PRs vs. human PRs; developer survey scores on "I understand what my agent did."

### Bet 3: Agent-Native API Primitives (v2 Surface)
**What:** A new API surface layer designed for agent consumption — not replacing REST/GraphQL but extending it with agent-optimized patterns: semantic queries, batch operations, context anchoring, and structured intent fields.

**Why now:** Every major AI coding tool (Copilot, Cursor, Devin, etc.) is building custom workarounds on top of GitHub's existing API. GitHub has the opportunity to standardize those patterns before the ecosystem fragments.

**How I'd measure it:** Third-party agent SDK adoption of new primitives; API call efficiency ratio (goals accomplished per API call) for agent vs. developer sessions; developer relations NPS among AI tooling partners.

---

## What I'd Do in the First 90 Days

**Days 1–30: Listen and map**
- Shadow 10 enterprise customers currently deploying agents against GitHub
- Audit the current API surface for agent friction points with engineering
- Map the competitive landscape: what are Cursor, Devin, and other agent-first tools working around?

**Days 31–60: Define and align**
- Draft a 12-month roadmap for the agent identity and trust layer
- Align with GitHub Security, Enterprise, and Developer Relations on priority and sequencing
- Identify the two or three quick wins that build credibility with engineering teams

**Days 61–90: Ship something**
- Drive the first agent-native feature to beta with a cohort of enterprise customers
- Establish the metrics baseline we'll use to measure agentic platform health going forward
- Publish a developer blog post or RFC that signals GitHub's direction to the ecosystem

---

## Why This Problem, Why Me

I run a three-tier AI command chain in production today: myself as Commander, Claude as OC/Strategist, and Moby as an autonomous agent operating via OpenClaw with Telegram integration. I have personally experienced every friction point that agents encounter when interacting with external APIs, managing state across sessions, and operating within trust boundaries set by a human principal.

I am not theorizing about agentic development. I am doing it.

That lived experience is what I bring to this roadmap.
