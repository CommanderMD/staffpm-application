# Eric Armstrong — Staff Product Manager Application

> *"The next version of GitHub isn't just a platform for developers. It's a platform for agents that build alongside them. I've been building that stack in production. Here's my thinking on where it goes next."*

---

## Why I'm Here

I'm a founder, systems architect, and developer who has spent the last decade building products at the intersection of infrastructure, AI, and developer tooling. I run four companies. I've shipped a live algorithmic trading platform with a webhook API and autonomous agent runtime. I've designed multi-tier SaaS products, token-gated access systems, and agentic orchestration pipelines — not as a manager of those efforts, but as the person who defined what to build, why, and how to measure success.

I'm applying for the Staff PM role on GitHub's core productivity team because the problem you're solving — making GitHub's platform robust, scalable, and designed for AI agents as first-class consumers — is the exact problem I've been living.

---

## My Thesis on GitHub's Agentic Platform

The shift GitHub is navigating is not just a feature upgrade. It's an architectural identity change.

For 15 years, GitHub's API was designed around one assumption: a human is on the other end of every call. Authentication flows, rate limits, event webhooks, and repository interactions were all optimized for developer-paced interaction — deliberate, contextual, and low-frequency relative to what's coming.

Agents don't work that way. They are:
- **High-frequency** — thousands of API calls per session, not dozens
- **Stateless by default** — they don't maintain context between calls the way a developer tab does
- **Goal-driven, not task-driven** — they need to understand intent, not just execute commands
- **Parallel** — multiple agents may be operating on the same repo simultaneously

This means GitHub's core platform needs to evolve across three layers:

**1. API Surface Redesign**
The current REST and GraphQL APIs were not built for agent consumption at scale. The next generation needs agent-native primitives: batched operations, semantic context endpoints, and structured intent signaling so agents can declare what they're trying to accomplish, not just what action they're taking.

**2. Trust and Identity at Agent Granularity**
OAuth scopes and personal access tokens were designed for humans and apps. Agents need a new identity layer — fine-grained, ephemeral, auditable, and revocable at the operation level. The platform needs to know not just *who* is calling, but *what agent*, *on whose behalf*, *toward what goal*, and *within what constraints*.

**3. Observability as a Product**
When agents are building software, developers need to *watch* them work, not just review their output. GitHub needs native agent activity feeds, decision logs, and rollback surfaces — not as DevOps tooling, but as first-class product features that make agentic workflows trustworthy and debuggable.

This is the roadmap I want to help build.

---

## What's In This Repo

| Folder | Contents |
|--------|----------|
| [`/resume`](./resume) | Resume (PDF + DOCX) |
| [`/product-thinking`](./product-thinking) | My take on GitHub's agentic platform evolution |
| [`/api-thinking`](./api-thinking) | How I'd approach the API surface for agent-scale consumption |
| [`/about`](./about) | Origin story and builder background |

---

## Quick Facts

| | |
|---|---|
| **Location** | Remote — Southaven, MS (U.S. Veteran) |
| **Contact** | ericarmstrong@techsmart1.com / (662) 417-8780 |
| **Companies** | Tech Smart Inc. · Rent Smart Inc. · Cloud Nexus LLC · Lion Investors Inc. |
| **Current AI Stack** | OMEGA v2 (live trading) · Moby (autonomous agent) · Claude (OC/Strategist) |
| **GitHub Handle** | [@CommanderMD](https://github.com/CommanderMD) |

---

## The Short Version

I am a developer who became a product leader because I kept seeing the gap between what engineers could build and what the market actually needed. I close that gap. I've done it across trading platforms, SaaS products, wireless infrastructure, real estate technology, and AI agent systems.

I'm not a traditional PM candidate. I'm a builder who thinks in systems, ships in production, and leads with clarity.

That's the kind of PM GitHub needs for this role.

---

*Resume, product thinking documents, and API notes are in the folders above.*
