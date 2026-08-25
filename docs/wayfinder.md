# DataDetective Product Wayfinder

The canonical, live version of this map is the GitHub issue labelled `wayfinder:map`.

## Destination

Produce a handoff-ready product specification, technical architecture, and implementation ticket plan for DataDetective. Continue past the first gate only if an evidence-driven e-commerce anomaly Investigation proves meaningfully more useful than a generic LLM plus notebook workflow.

## Notes

- Domain: e-commerce business Metric anomaly Investigation.
- Optimize for Agent engineering hiring signal (60%) and real product value (40%).
- Planning only; implementation starts after this map reaches its destination.
- The product must be standalone, publicly usable, read-only, evidence-preserving, and reproducible.
- Delivery constraints: 8-12 weeks solo, about CNY 500/month, Java/Spring Boot business backend, and Python Agent runtime.
- Canonical domain language lives in [CONTEXT.md](../CONTEXT.md).
- Recruitment requirements live in [Agent engineer hiring requirements](agent-engineer-job-requirements.md).

## Decisions so far

- The broad claim "ask why a business metric changed" is already occupied, most directly by ThoughtSpot Spotter.
- DataDetective proceeds only as a benchmark-first experiment for raw exports, focused on explicit falsification, rerunnable evidence, and separating contribution from causality.
- See [Existing Alternatives to DataDetective](research/existing-alternatives.md).
- No reviewed public dataset combines rich e-commerce data, permissive commercial reuse, and known root-cause labels. The scored benchmark core must use deterministic synthetic cases with hidden intervention manifests; UCI Online Retail can provide real-data holdouts.
- See [Benchmark Data and Case Research](research/benchmark-data-and-cases.md).
- No viability decision has been made yet.

## Not yet specified

- The complete Investigation experience: clarification, progress, evidence inspection, challenge flow, and report interaction.
- The detailed e-commerce domain model and causal limits.
- Agent state graph, tool contracts, context policy, memory, recovery, and human approval points.
- Java/Python module boundaries, event contracts, persistence ownership, and real-time delivery.
- Frozen evaluations, deterministic checks, model judges, regression gates, and public scorecards.
- Data isolation, code execution sandboxing, retention, deletion, prompt-injection defenses, and resource budgets.
- Whether database or warehouse connectors belong after the upload workflow is validated.
- Deployment, observability, cost controls, release strategy, and implementation sequence.

## Out of scope

- A generic chat-with-Excel assistant, dashboard builder, or arbitrary BI copilot.
- Mutating, cleaning, or repairing source data.
- Claiming support for every industry before the e-commerce model is validated.
- Production implementation before the product and architecture decisions are resolved.
