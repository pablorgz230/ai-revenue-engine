# Architecture Decisions

This document records important technical and architectural decisions
made throughout the development of the AI Revenue Engine.

The purpose is to document not only what was chosen, but also why it
was chosen and what consequences the decision has for the project.

---

## ADR-001 — Synthetic Data

### Status

Accepted

### Decision

The project will use synthetic sales data for development, testing
and evaluation.

No confidential, proprietary or employer-owned data will be used.

### Reason

The project is developed independently as a personal portfolio project.
Using synthetic data allows the system to be designed and tested without
exposing private business information.

### Consequences

The system should be designed so that synthetic data can eventually be
replaced by real business data without requiring major changes to the
core architecture.

---

## ADR-002 — Local-First Development

### Status

Accepted

### Decision

The project will use local infrastructure whenever it is technically
reasonable to do so.

Examples include:

- Local LLM inference
- Local databases
- Local automation
- Local development environments

Cloud services and APIs will be introduced when they provide a clear
technical or learning advantage.

### Reason

A local-first approach reduces unnecessary costs, provides greater
control over development data and allows experimentation with local AI
models.

It also provides an opportunity to understand both local and cloud-based
AI architectures.

### Consequences

The system may require additional local configuration and hardware
resources.

The architecture should remain flexible enough to use cloud services
when required.

---

## ADR-003 — Human-in-the-Loop for External Actions

### Status

Accepted

### Decision

AI systems must not independently perform sensitive external actions
without human approval.

This includes actions such as:

- Sending external communications
- Making significant CRM changes
- Executing potentially destructive operations

### Reason

The project is intended to demonstrate practical AI automation rather
than unrestricted autonomous execution.

Human approval provides a safety layer and allows AI-generated actions
to be reviewed before execution.

### Consequences

Some workflows will contain an explicit approval stage.

The system must distinguish between actions that can be performed
automatically and actions that require human confirmation.

---

## ADR-004 — Model-Agnostic Architecture

### Status

Accepted

### Decision

The system should not depend on a single LLM provider.

The architecture will allow different models to be used for different
tasks where practical.

Potential providers include:

- OpenAI
- Google Gemini
- Local models
- Other compatible LLM providers

### Reason

Different models have different capabilities, costs, latency and
hardware requirements.

A model-agnostic architecture allows the project to compare these
trade-offs and eventually implement task-based model routing.

### Consequences

The project will need a consistent interface for interacting with
different models.

Model-specific functionality should be isolated where possible.

---

## ADR-005 — AI Decisions Must Be Traceable

### Status

Accepted

### Decision

Important AI-generated decisions should be logged and traceable.

Where practical, the system should record:

- Model used
- Input
- Output
- Timestamp
- Cost
- Latency
- Result
- Relevant evaluation data

### Reason

AI systems should be evaluated based on observable results rather than
subjective impressions.

Traceability will allow the project to analyse model performance,
identify failures and compare different approaches.

### Consequences

The system will require persistent logging and eventually a dedicated
data layer for AI runs and evaluations.

---

## ADR-006 — Evaluation Before Optimization

### Status

Accepted

### Decision

AI components should be evaluated against labelled data before
optimizing them for cost, latency or model selection.

### Reason

A cheaper or faster model is not necessarily better if it produces
significantly worse commercial decisions.

The project should first establish whether a component works and then
optimize its performance.

### Consequences

Evaluation datasets and measurable metrics will be introduced early
in the project.

---

## ADR-007 — Agentic Layer as a Final Integration Layer

### Status

Accepted

### Decision

Hermes Agent / OpenClaw will be introduced after the core Revenue Engine
components have been developed.

The agentic layer will interact with existing capabilities through
defined tools rather than replacing the underlying system.

### Reason

The goal is to understand agentic architecture rather than simply use
an agent framework as a black box.

Building the underlying tools first will make it possible to evaluate
how an agent interacts with databases, CRM systems, research tools,
files and business workflows.

### Consequences

The project will initially be developed without Hermes Agent / OpenClaw.

The agentic layer will be introduced during the v0.9 phase.

---

## Future Decisions

Additional architectural decisions will be documented here as the
project evolves.