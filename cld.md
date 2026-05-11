# Claude by Anthropic
> Next Generation AI Assistant · May 2026

---

## Table of Contents

1. [Who Is Anthropic?](#1-who-is-anthropic)
2. [Model Timeline](#2-model-timeline)
3. [Model Tiers & Selection Guide](#3-model-tiers--selection-guide)
4. [Core Capabilities & Benchmarks](#4-core-capabilities--benchmarks)
5. [Claude Code](#5-claude-code)
6. [Claude Cowork](#6-claude-cowork)
7. [Claude Design](#7-claude-design)
8. [MCP & Deployment Infrastructure](#8-mcp--deployment-infrastructure)
9. [Plans, Access & Enterprise Controls](#9-plans-access--enterprise-controls)
10. [SRE Use Cases — Incident Response & Observability](#10-sre-use-cases--incident-response--observability)
11. [SRE Use Cases — Automation, Code Review & Documentation](#11-sre-use-cases--automation-code-review--documentation)

---

## 1. Who Is Anthropic?

**Founded:** 2021 by 7 former OpenAI researchers and executives
**CEO:** Dario Amodei — ex OpenAI VP of Research
**President:** Daniela Amodei — ex OpenAI VP of Operations

**Mission:** Build the world's safest and most capable AI. Safety is a first-class engineering constraint, not a post-hoc filter.

### Safety Methodology — Constitutional AI (CAI)

Training is a two-phase process:

1. **Supervised learning**: The model self-critiques and revises its own responses against a 23,000-word "constitution" of guiding principles.
2. **RLAIF**: Reinforcement learning from AI feedback, using the same principles as a reward signal.

This produces more robustly aligned models than standard RLHF. Resistance to misuse is a training primitive, not a layer added on top after the fact.

### Cloud & Infrastructure Partners

| Partner | Details |
|---------|---------|
| **Amazon Bedrock** | Up to $4B investment; global + regional endpoints |
| **Google Vertex AI** | Up to 1M TPUs; global, multi-region, and regional endpoints |
| **Microsoft Azure Foundry** | Only cloud offering both Claude and GPT frontier models |
| **SpaceX** | Multi year contract offering Anthropic more compute |

**Compute:** 220,000 Nvidia GPUs · 300MW energy capacity

**Policy:** Sales are restricted to entities majority-owned by Chinese, Russian, Iranian, or North Korean entities.

---

## 2. Model Timeline

| Date | Model | Key Milestone |
|------|-------|---------------|
| Mar 2023 | Claude 1 & Claude Instant | Limited access — research preview |
| Jul 2023 | Claude 2 | Public launch — improved reasoning, 100K context window |
| Mar 2024 | Claude 3: Haiku, Sonnet, Opus | Multi-modal vision; tiered capability model introduced |
| Jun 2024 | Claude 3.5 Sonnet | Computer use public beta; Artifacts feature launched |
| May 2025 | Claude 4: Opus + Sonnet | MCP connectors, Files API, code execution tool; Claude Code GA |
| Nov 2025 | Claude Opus 4.5 | 65% fewer tokens on long-horizon tasks; agent team; effort control API parameter |
| Feb 5 2026 | Claude Opus 4.6 | 1M token context; 14.5hr task horizon (METR); Claude in PowerPoint |
| Feb 17 2026 | Claude Sonnet 4.6 | 72.5% OSWorld computer use; major prompt injection improvements; default model across all plans |
| Apr 16 2026 | Claude Opus 4.7 | **Current frontier** — best coding, reasoning, agentic, and vision performance |

### Current Production Model Strings

```
claude-opus-4-7
claude-sonnet-4-6
claude-haiku-4-5-20251001
```

Anthropic uses pinned, dated model strings so production deployments don't break on model updates.

---

## 3. Model Tiers & Selection Guide

| Model | Tag | Context Window | Pricing | Best For |
|-------|-----|---------------|---------|----------|
| **Opus 4.7** | Frontier | 1M tokens | Premium | Complex agents, production code, multi-step orchestration, frontier research |
| **Sonnet 4.6** | Default | 1M tokens (beta) | $3 / $15 per 1M tokens | Daily dev tasks, code review, knowledge work, CI/CD — default on all plans |
| **Haiku 4.5** | Speed | 200K tokens | Lowest in family | High-volume, latency-sensitive workloads, classification, rapid triage |

### Benchmark Highlights

- Sonnet 4.6 preferred over the prior frontier model (Opus 4.5) by **59%** of Claude Code users
- Sonnet 4.6 hits **72.5%** on the OSWorld computer use benchmark
- Opus 4.7 hits **98.5%** visual acuity benchmark on computer use

### Context Window at Scale

- **200K tokens** ≈ 500+ pages of text
- **1M tokens** ≈ 75,000 lines of code (more than the entire Lord of the Rings trilogy)

> **Note on context rot:** More context isn't automatically better. As token count grows, accuracy and recall degrade. Curating what goes into context is as important as the window size. All current models track their remaining token budget in-session to pace long tasks without silent degradation.

---

## 4. Core Capabilities & Benchmarks

| Metric | Value | Notes |
|--------|-------|-------|
| Context window | 1M tokens | Opus 4.7 & Sonnet 4.6 |
| Task horizon | 14.5 hours | 50% success rate — METR eval, Opus 4.6 |
| Computer use | 72.5% | OSWorld benchmark — Sonnet 4.6 |
| Visual acuity | 98.5% | Computer use benchmark — Opus 4.7 |

### Reasoning & Extended Thinking

- Hybrid reasoning mode with extended thinking, interleaved between tool calls on all Claude 4 models
- Strong performance on deductive logic, multi-step planning, and structured problem-framing
- Catches its own logical faults during the planning phase before execution begins — reduces iteration cycles

### Agentic Execution

- Orchestrates teams of specialized **subagents in parallel** within a single session
- Models track remaining token budget throughout a session to pace long tasks rather than silently degrading
- **Automatic context compaction** — summarizes earlier context to extend sessions indefinitely (Infinite Chats)

### Computer Use

- Interprets screen content and simulates keyboard/mouse input to navigate any desktop application
- Grew from <15% to 72.5% OSWorld benchmark performance across 2025–2026
- Handles spreadsheets, web forms, file management, and multi-app workflows at near-human performance

### Vision & Multimodal

- Up to **600 images or PDF pages** per request on 1M-token models (100 on 200K-token models)
- Reads architecture diagrams, dashboards, runbooks, and screenshots natively — no OCR preprocessing needed
- All current models: text + image input, text output, multilingual, vision — no capability tiers within a model

---

## 5. Claude Code

> A command-line AI agent that reads/writes files, executes shell commands, runs tests, and commits to Git natively in the terminal. Released for research preview in Feb 2025, general release May 2025. 

### Key Features

- **Subagents**: Spawns specialized parallel sub-agents within a single session for concurrent multi-task orchestration.
- **MCP Integration**: Connects to linters, test runners, CI/CD, Jira, GitHub, Docker, Kubernetes, Datadog, and thousands more via the MCP ecosystem.
- **Code Security**: Reviews entire codebases to identify vulnerabilities (launched Feb 2026).
- **CI/CD Native**: TypeScript and Python SDKs; runs inside GitHub Actions or any pipeline automation without a UI.

### Quick Start

```bash
# Install globally
npm install -g @anthropic-ai/claude-code

# Run interactively in your repo
claude

# Non-interactive / CI pipeline
claude --print "Fix failing tests"

# Add an MCP server
claude mcp add --transport http datadog https://mcp.datadoghq.com
```

### Performance

| Metric | Value | Notes |
|--------|-------|-------|
| User preference: Sonnet 4.6 vs Opus 4.5 | 59% | In Claude Code sessions |
| Token reduction on long-horizon coding tasks | 65% | Opus 4.5 vs Sonnet 4.5 |
| SWE-bench improvement at max effort (Opus 4.5) | +4.3pp | Over Sonnet at the same effort level |

Available on **Pro, Max, Team, and Enterprise** plans.

---

## 6. Claude Cowork

> GUI equivalent of Claude Code. Same agentic architecture, built for knowledge workers without requiring terminal access. Research preview Jan 2026, enterprise GA Feb 2026. Mac, Windows & web. Built by Claude Code itself in two weeks.

### Key Features

- **File System Access**: Reads, writes, and edits files in user-specified folders; completes full multi-step tasks end-to-end, not just answering questions
- **Scheduled Tasks**: Define a recurring task once (e.g. pull metrics every Friday, weekly Slack digest, scan email each morning) and Claude handles it going forward
- **Browser Integration**: Claude for Chrome connector completes tasks across all open browser tabs without window switching
- **Enterprise Controls**: role-based access via SCIM, group spend limits per team, OpenTelemetry observability, usage analytics dashboard

### Enterprise Connectors (Sample)

Google Drive · Gmail · DocuSign · FactSet · AWS Marketplace · Honeycomb · n8n · Fellow.ai · Zoom · and hundreds more via MCP

### The Anthropic Product Evolution

| Product | Who It's For |
|---------|-------------|
| **Claude Chat** | Answers questions in conversation |
| **Claude Code** | Executes full tasks for developers |
| **Claude Cowork** | Executes full tasks for everyone else |

---

## 7. Claude Design

> Conversational visual creation tool from Anthropic Labs. Launched April 17, 2026 in research preview for Pro, Max, Team, and Enterprise. Powered by Claude Opus 4.7. Describe what you want → Claude generates interactive prototypes, slides, landing pages, and marketing assets. Output is live HTML — not a static image.

### Key Features

- **Brand System Ingestion**: Reads your codebase and Figma files to extract your design system and applies it automatically to every new project.
- **Iterative Refinement**: Inline comments on specific elements, direct text editing, adjustment sliders for spacing/color/layout. Changes applied consistently across the full design.
- **Claude Code Handoff**: Packages completed designs into a handoff bundle; one instruction to Claude Code generates production React/Next.js.
- **Export Anywhere**: PPTX, PDF, standalone HTML, internal org URL, or direct to Canva (strategic Canva partnership announced at launch).

### What It Creates

- UI prototypes & wireframes
- Pitch decks & presentations
- Landing pages (exportable as standalone HTML)
- Marketing one-pagers
- Product mockups
- Code-powered prototypes (voice, 3D, shaders)
- Brand explorations & variations

### Market Context

- Dropped Figma stock **7%** on launch day — announced 3 days after Anthropic's CPO stepped down from Figma's board
- Canva partnership: Claude Design is the upstream creative engine feeding into Canva's collaborative ecosystem
- **Enterprise:** disabled by default — admins enable under *Organization Settings → Capabilities → Anthropic Labs*

---

## 9. Plans, Access & Enterprise Controls

| Plan | Key Features |
|------|-------------|
| **Free** | claude.ai web & mobile · Sonnet 4.6 default · File creation & connectors · Context compaction |
| **Pro** | 5× usage vs Free · Claude Code · Claude Design (research preview) · All model tiers |
| **Max** | 5–20× higher usage limits · Claude for Chrome · Opus models without caps · Priority access |
| **Team** | Shared workspaces · Team usage management · MCP connectors shared across team |
| **Enterprise** | SSO (SAML/SCIM) · Audit logs · Compliance API · Regional data routing · Custom rate limits · Role-based access · OpenTelemetry observability · Group spend limits · **No training on your data** |

### Security

- **Prompt injection resistance**: Sonnet 4.6 is a major improvement over its predecessor, particularly relevant for agentic workflows and computer use that touch untrusted external data sources
- **No training on Team/Enterprise data**: Contractual guarantee that data is not used to train models.
- **Constitutional AI**: Resistance to misuse is a training primitive, not a post-hoc filter
- **Regional routing**: Bedrock and Vertex AI guarantee geographic data routing for compliance requirements

---

## 10. SRE Use Cases — Incident Response & Observability

### Incident Response

**Log & trace analysis at scale**
Ingest up to 75,000 lines of raw logs, stack traces, or distributed traces in a single request. No pre-filtering or manual grepping required before you can ask a useful question.

**Runbook-assisted triage**
Load your runbook library alongside the incident data. Claude cross-references both simultaneously and surfaces relevant remediation steps for the specific failure mode — not generic advice.

**Post-mortem generation**
Generate a structured post-mortem draft from a raw Slack thread, alert history, or PagerDuty timeline in minutes. Edit rather than write from scratch.

**MCP-connected live triage**
With PagerDuty and Datadog MCP servers configured, Claude pulls the live alert, cross-references historical incidents, proposes a fix, and can trigger a patch deployment — without leaving the terminal.

**On-call handoff summaries**
Summarize the last 8 hours of alerts, deployments, and Slack discussion into a structured handoff doc at shift change. Consistent format every time.

### Observability

**Query generation**
Generate PromQL, LogQL, or Splunk SPL from plain-English descriptions. No syntax lookup, no trial and error. Explain what existing queries do and suggest optimizations.

**Alert noise analysis**
Summarize weeks of alert history to identify noise vs. signal patterns, flapping alerts, and correlated failure modes across services.

**SLO drafting**
Draft SLO definitions from service behavior descriptions and error budget history. Generate the PromQL recording rules to track them.

**Dashboard interpretation**
Feed Claude a screenshot of a Datadog or Grafana dashboard and get a written summary of what it shows, anomalies detected, and recommended next steps.

**Capacity planning**
Analyze historical metrics, project growth curves, and summarize into a report with recommended headroom thresholds.

### SRE MCP Integrations Available Today

`PagerDuty` `Datadog` `Prometheus` `Grafana` `GitHub` `Jira` `Slack` `Kubernetes` `Docker` `AWS CloudWatch` `Honeycomb` `Splunk` `New Relic` `Linear`

---

## 11. SRE Use Cases — Automation, Code Review & Documentation

### Infrastructure & Code Review

**Terraform / Helm / Kubernetes YAML**
Review for security misconfigurations, anti-patterns, and config drift across the full file set — not just the diff. Load the entire IaC repo in one context window.

**Codebase comprehension**
Explain what an unfamiliar 5,000-line module does before you're paged on it at 2am. Map build system dependencies and understand how changes will propagate before touching shared libraries.

**Automated CI/CD review**
Claude Code runs inside GitHub Actions — reviews PRs, flags regressions, generates test cases, and suggests fixes without a human in the loop.

### Scripting & Automation

**Operational script generation**
Generate Python, Bash, or Go scripts with edge case handling specified upfront. Claude writes the retry logic and error handling you'd otherwise skip under pressure.

**Query & filter tools**
Write and explain regex, `jq` filters, `awk` pipelines. Translate between query languages (PromQL ↔ LogQL ↔ Splunk SPL). Explain what existing queries do before you modify them.

**Autonomous deploy agent**
Claude Code + MCP: query logs on failure, diagnose root cause, recommend a fix, and trigger a patch deployment — all within governed, auditable workflows.

### Knowledge & Documentation

**Runbook generation**
Auto-generate runbooks from code or architecture diagrams. Keep them in sync as services evolve. No more stale docs that haven't been touched since the service was written.

**ADR writing**
Write Architecture Decision Records from Slack threads, Jira tickets, or design review notes in a consistent format across the team.

**Unified knowledge query**
Ask Claude questions across your entire internal wiki, runbook library, and past incident reports simultaneously — one query instead of five browser tabs.
