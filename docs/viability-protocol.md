# DataDetective Viability Protocol

Status: approved for prototype work

This protocol decides whether DataDetective is more useful than a strong general-purpose file-analysis workflow for investigating e-commerce metric changes. It is a product gate, not a model leaderboard.

## Decision question

Given the same raw business data and the same business question, can DataDetective produce a more reliable, reproducible, and honest investigation than a general-purpose LLM plus Python/Notebook?

The target user has an anomaly to investigate, a few CSV/Excel/SQLite exports, and little or no SQL knowledge. The product does not need to discover anomalies in this experiment.

## Scope of one investigation

Input:

- One natural-language question, such as: "Why did GMV fall in the last seven days?"
- One to three CSV, Excel, or SQLite sources
- An optional data dictionary supplied to both systems
- A fixed comparison window and baseline window, when the question provides them

Output:

- Metric definition and analyzed population
- Data-quality checks and limitations
- Ranked falsifiable hypotheses
- Executed evidence for each tested hypothesis
- Findings with evidence references and uncertainty labels
- Missing data required to distinguish unresolved hypotheses
- Re-runnable SQL/Python and input snapshot metadata

Out of scope:

- Proactive anomaly detection
- Data cleaning or mutation
- Dashboard construction
- Internet research
- Hidden-answer lookup
- Human-written SQL that assists either system

## Evaluation corpus

### Core scored set

Build 24 deterministic synthetic relational cases: eight case families with three seeds and varied effect sizes or distractors each.

1. Traffic decline by channel, device, or region
2. Checkout conversion decline after cart
3. Average-order-value composition shift
4. Price or discount intervention
5. Cancellation spike by cohort
6. Data-quality defect such as duplicate rows, broken joins, timezone shift, or truncated partition
7. Multi-cause incident with unequal contributions
8. Insufficient-evidence or no-anomaly control

Each case has public input data and a public question. Its private intervention manifest records the affected mechanism, entity, segment, time window, expected contribution, distractors, acceptable aliases, and whether the available evidence supports causation, association only, or no conclusion.

The generator must be deterministic from a pinned commit and seed. Interventions are applied at the lowest meaningful grain, such as sessions or orders, rather than by directly changing an aggregate metric.

### Real-data holdout

Use six UCI Online Retail-derived cases to test realistic transaction distributions, evidence execution, arithmetic, provenance, and unsupported claims. Do not score natural root-cause recall on these cases: the dataset does not provide reliable business intervention labels.

Olist, Retailrocket, and Google GA4 sample data remain optional research or connector checks. They are not required to run the public product and are not part of the permissively redistributable scored core.

## Systems under comparison

### Baseline A: general-purpose file-analysis workflow

- Same model family and model version as DataDetective
- Same input files, data dictionary, question, and task-specific context
- One investigation prompt that asks the model to inspect the files and use Python/Notebook tools
- No human-written SQL, hypothesis list, field correction, or manual interpretation
- Same model token budget and maximum wall-clock budget

### System B: DataDetective

- Java/Spring Boot is not required for the viability experiment; the Agent runtime may be run directly
- The Agent may plan, call typed analysis tools, preserve state, discard hypotheses, replan, and ask up to three clarification questions
- The user may confirm metric definitions or table relationships, but may not write analysis code
- The private manifest remains inaccessible during execution
- All executed tools and outputs are recorded

Both systems receive the same permitted clarification answers. Runs are repeated from clean state with a fixed random seed where the model supports it.

## Procedure

1. Freeze the generator commit, case seeds, input hashes, model version, system prompts, tool budgets, and pricing table.
2. Run every core case against Baseline A and DataDetective from clean state.
3. Run every real-data holdout against both systems.
4. Store raw traces, executed code, tool outputs, report JSON, wall-clock time, token usage, and estimated model cost.
5. Score automatically wherever possible. Use a blinded reviewer only for claim classification or ambiguous aliases.
6. Re-run every cited SQL/Python artifact against the frozen input snapshot.
7. Publish aggregate results and representative failures. Never publish private manifests for unreleased cases.

## Metrics

### Primary metrics

**Top-3 hypothesis recall**

For synthetic intervention cases, a manifest cause is recalled when an accepted hypothesis appears in the top three and matches the mechanism, affected segment, and time window within the case tolerance. Multi-cause cases require every material cause to be present for full case credit.

**Unsupported causal claim rate**

Count causal statements that the case manifest and executed evidence do not license, divided by all causal statements in the report. Contribution and association statements are not causal statements, but they must still be labeled accurately.

**Evidence traceability**

Every final Finding must reference an Evidence record whose tool call actually executed and whose result contains the cited value or observation.

### Secondary metrics

- Evidence correctness: cited values match frozen assertions within declared tolerance.
- Replay success: every cited computation regenerates its result from the input snapshot.
- Nonexistent-field references: count of fields absent from the supplied schema.
- Data-quality precedence: whether a defect is surfaced before a business explanation is asserted.
- Honest abstention: whether insufficient-evidence cases request the missing data or state non-identifiability.
- User completion time: time from upload to a usable first report, including clarification.
- Model cost: model input/output cost for one standard investigation.
- Tool count, token count, wall-clock latency, and failure/retry count.

## Pass gates

DataDetective passes the viability gate only if all hard gates hold on the frozen run:

- Top-3 hypothesis recall is at least 70% on the synthetic core.
- 100% of final Findings have traceable executed Evidence.
- Zero nonexistent-field references.
- 100% replay success for cited computations.
- Unsupported causal claim rate is no higher than 10%.
- P95 model cost is no higher than CNY 1 per standard investigation.
- P95 time to a usable first report is no higher than 10 minutes for non-technical users.
- On at least one primary metric, DataDetective improves on Baseline A by at least 15 percentage points without failing any hard gate or causing a greater-than-10% relative regression in another primary metric.

The comparison is invalid if one system receives more usable context, more manual help, or a larger model/tool budget.

## Decision rules

### Continue

Proceed to the full Investigation product and architecture specification only when every pass gate holds and the improvement over Baseline A is meaningful.

### Narrow the scope

Reframe around the strongest validated case family if the system is reliable for a narrow task but fails to generalize across the full corpus. The new product claim must name the supported metric or investigation pattern.

### Stop

Stop the current product direction if DataDetective does not beat Baseline A, cannot maintain evidence traceability, repeatedly overstates causality, or cannot stay within the cost and time limits.

## Known limitations

- Synthetic cases measure the investigation contract and failure behavior; they do not prove performance on every retailer.
- UCI holdouts test realistic transaction distributions but do not establish natural root causes.
- LLM nondeterminism means the protocol must report repeated-run variance where possible.
- A passing benchmark is necessary, not sufficient, evidence of product-market value.
