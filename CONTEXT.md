# DataDetective

DataDetective investigates unexpected changes in e-commerce business metrics. Its language separates what was observed, what might explain it, and what the available data can actually support.

## Language

**Investigation (调查)**:
A bounded inquiry into why a Metric exhibited an Anomaly, preserving the hypotheses tested and evidence produced.
_Avoid_: Analysis session, chat, report job

**Metric (指标)**:
A business quantity with an explicit definition, population, aggregation, and time window.
_Avoid_: Number, KPI when the calculation is unspecified

**Anomaly (异常)**:
An observed change in a Metric that the user has chosen to investigate; it does not imply an error or known cause.
_Avoid_: Problem, incident, root cause

**Hypothesis (假设)**:
A falsifiable candidate explanation for an Anomaly.
_Avoid_: Guess, conclusion

**Evidence (证据)**:
A reproducible result derived from an identified data source that supports, weakens, or leaves a Hypothesis unresolved.
_Avoid_: Insight, model opinion

**Finding (发现)**:
A user-facing statement justified by Evidence and qualified by its remaining uncertainty.
_Avoid_: Root cause unless causality has actually been established

**Investigation Report (调查报告)**:
The durable outcome of an Investigation, containing metric definitions, data-quality limits, tested hypotheses, evidence, reproducible analysis, findings, and unresolved questions.
_Avoid_: Dashboard, summary, answer
