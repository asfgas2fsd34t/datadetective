# DataDetective

DataDetective is an evidence-driven AI agent that investigates why an e-commerce business metric changed unexpectedly.

Instead of returning a plausible explanation in chat, it maintains falsifiable hypotheses, executes read-only analysis tools, links every finding to reproducible evidence, and records what remains uncertain.

## Product boundary

DataDetective is designed for product, operations, and marketing teams working with CSV, Excel, or SQLite data. It is not a generic spreadsheet chatbot, dashboard builder, or autonomous data-cleaning tool.

The first product gate asks one question: **does an investigation workflow outperform a general-purpose LLM plus notebook on repeatable e-commerce anomaly cases?** Production implementation starts only after that gate passes.

## Planned architecture

- **Java / Spring Boot:** identity, authorization, investigation lifecycle, persistence, asynchronous jobs, and streaming progress
- **Python / LangGraph:** agent state graph, hypothesis planning, tool execution, context policy, recovery, and evaluation
- **PostgreSQL:** users, investigations, durable state, evidence metadata, and audit records
- **Redis:** transient execution state, rate limits, and stream coordination
- **Object storage:** uploaded datasets and generated investigation artifacts
- **SSE:** live investigation progress and human approval points

## Initial viability criteria

- Top-3 hypothesis recall of at least 70% on a frozen benchmark
- Every finding traceable to executed evidence
- Zero references to nonexistent fields
- Reproducible investigation runs
- Standard model cost no higher than CNY 1 per investigation
- A non-technical user can complete a first investigation within 10 minutes

## Current status

The project is in product validation. The [Wayfinder issue](https://github.com/asfgas2fsd34t/datadetective/issues/1) tracks research, prototype, and go/no-go work. No production scaffold is included before viability is established.

## Documentation

- [Domain language](CONTEXT.md)
- [Agent engineer hiring requirements](docs/agent-engineer-job-requirements.md)
- [Product wayfinder](docs/wayfinder.md)
- [Viability protocol](docs/viability-protocol.md)
