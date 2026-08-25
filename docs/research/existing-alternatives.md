# Existing Alternatives to DataDetective

Research date: 2026-08-25

## Research question

What can current general-purpose LLM data-analysis products, notebook assistants,
spreadsheet copilots, and established analytics tools already do for business-metric
anomaly investigation? Where, if anywhere, could a hypothesis-driven,
evidence-preserving agent provide a measurable advantage rather than a cosmetic
workflow difference?

## Executive conclusion

The broad product claim "ask why a business metric changed and let an AI investigate"
is already occupied. It is not a defensible differentiation for DataDetective.

The strongest direct overlap is ThoughtSpot Spotter. Its first-party documentation uses
the example "Why did my sales drop last month?", says it identifies key attributes,
runs change analysis, and returns an analysis plan, contribution visualizations, and a
narrative of key drivers. ThoughtSpot also describes Spotter 3 as able to plan, reason,
iterate, and execute complex analysis. Microsoft Excel Copilot now plans multi-step
work before executing it, while Power BI Copilot, Databricks Genie, and ThoughtSpot
all combine natural-language analysis with governed semantic context.

An evidence ledger by itself is also insufficient differentiation. General-purpose
data analysis already exposes executable code, notebook products preserve code and
outputs, and BI products expose underlying queries or governed semantic definitions.

There is a narrower, testable product thesis:

> For a non-technical operator with raw CSV/Excel/SQLite exports and no prepared
> semantic model, DataDetective can investigate a known metric anomaly more reliably
> than a general-purpose file-analysis assistant by explicitly generating competing
> hypotheses, trying to falsify them, preserving every executed result, separating
> observed contribution from causal claims, and reporting what the available data
> cannot establish.

That thesis is not established by the product concept. It must be won in a benchmark.
The Wayfinder should therefore treat DataDetective as a benchmark-first experiment,
not proceed directly to full product architecture.

## Capability comparison

| Category | What first-party sources show is already available | Consequence for DataDetective |
|---|---|---|
| General-purpose LLM data analysis | ChatGPT accepts common tabular files, writes and executes Python, creates tables/charts, and lets the user inspect the analysis/code. | "Upload a file and chat with your data" and "show the generated code" are commodity capabilities. |
| Spreadsheet copilots | Excel Copilot analyzes data, generates formulas, creates charts and PivotTables, identifies trends/outliers, can ground web research with citations, and generates a reviewable plan before workbook edits. | A spreadsheet-native plan/act loop already exists. A separate web app needs substantially better investigation quality, not a cleaner chat UI. |
| BI copilots | Power BI Copilot answers ad-hoc questions, analyzes visuals, summarizes reports, generates DAX/report content, and can use verified answers defined by model authors. | Natural-language analytics plus governed answers is established. DataDetective cannot claim novelty from RAG, metric definitions, or citations alone. |
| Data-platform agents | Databricks Genie supports natural-language data questions grounded in governed organizational data. Analysts configure datasets, sample SQL, instructions, metrics, business rules, and trusted assets. Genie Code supports agentic tasks in the workspace. | Enterprise competitors have a large advantage when a semantic layer already exists. DataDetective's plausible entry point is data with no such setup. |
| Agentic analytics / root-cause analysis | ThoughtSpot Spotter directly supports "why" questions, produces an analysis plan, runs change analysis, decomposes driver contributions, and summarizes findings. Spotter 3 is described as planning, reasoning, iterating, and executing complex analysis. | "Hypothesis-driven anomaly investigation" overlaps an existing product category. The project must beat a baseline on reliability or serve an underserved input/setup boundary. |
| Notebook assistants | Modern data platforms pair AI coding assistance with SQL/Python notebooks, preserving executable cells and outputs. A human analyst can branch, inspect, rerun, and revise the investigation. | Reproducibility and an audit trail are expected baseline behavior. DataDetective's value would have to be autonomous investigation quality for users who cannot drive a notebook themselves. |

## Findings by alternative

### 1. ChatGPT data analysis

OpenAI documents a file-analysis workflow in which ChatGPT can analyze uploaded Excel,
CSV, PDF, and JSON files, create static or interactive charts, and perform operations
such as regressions and scenario simulations. The user can inspect the analysis to see
how the work was performed and copy the generated code.[1]

Decision relevance:

- Raw-file onboarding is already available to a mass-market user.
- Python execution and chart generation are not differentiators.
- Visible code partially satisfies reproducibility, although it does not by itself
  create a structured hypothesis/evidence model.
- The primary baseline for the intended no-setup user is therefore ChatGPT plus a
  carefully written investigation prompt, not a traditional dashboard.

### 2. Microsoft Copilot in Excel

Microsoft says Copilot in Excel uses native workbook tools including tables, charts,
PivotTables, and formulas. It can summarize data, identify insights such as trends and
outliers, and use web search with citations. For multi-step editing, Copilot first
generates a plan for review and confirmation, then executes the steps while leaving the
workbook editable.[2]

Decision relevance:

- Planning followed by tool execution is already present in the spreadsheet where many
  target users work.
- Editable formulas, pivots, and charts provide stronger user verification than a prose
  report with hidden computations.
- DataDetective must preserve equally inspectable artifacts and demonstrate superior
  anomaly-investigation behavior. Moving the same workflow to a new UI is regression,
  not differentiation.

### 3. Microsoft Copilot in Power BI

Power BI Copilot supports chat-based ad-hoc analysis, questions over semantic models,
report and topic summaries, visual analysis, report creation, and DAX generation.[3]
Microsoft explicitly recommends preparing semantic models with AI data schemas, AI
instructions, and verified answers to reduce ambiguity and improve relevance and
accuracy. It also warns that these controls cannot guarantee identical output because
the system is nondeterministic.[4]

Decision relevance:

- Business semantics are a prerequisite to reliable answers, not a detail that can be
  recovered perfectly from column names.
- DataDetective's no-setup promise creates a structural quality disadvantage. It needs
  a metric-definition clarification step and must abstain when joins or metric meaning
  are ambiguous.
- "Verified answers" show that mature products mix deterministic, author-approved
  artifacts with generated analysis. A pure autonomous-agent approach is unlikely to
  be more trustworthy.

### 4. Databricks Genie

Databricks describes Genie as a family of natural-language data experiences grounded in
organizational data and governed through Unity Catalog. Genie Agents are domain-specific
environments configured by analysts using trusted datasets, metrics, business rules,
sample SQL, instructions, and trusted assets. Databricks recommends adding these assets
before sharing an agent to improve response accuracy.[5][6]

Decision relevance:

- The enterprise path is semantic preparation plus governance, not unrestricted
  autonomous exploration of arbitrary tables.
- DataDetective should not compete where an organization already has curated metrics
  and a warehouse assistant.
- Upload-first investigation can still serve a different moment: an operator has an
  export and a question but no analytics team, modeled dataset, or configured agent.
  This convenience only matters if accuracy remains acceptable.

### 5. ThoughtSpot Spotter

ThoughtSpot is the closest direct competitor. Its official "Why questions" documentation
states that a user can ask questions such as "Why did my sales drop last month?" Spotter
identifies attributes, uses a change-analysis engine, presents its analysis plan, breaks
down contributions with visualizations, and summarizes key drivers.[7] ThoughtSpot's
product overview also says Spotter 3 can plan, reason, iterate, and execute complex
analysis.[8]

The capability is bounded rather than universal. The documented "why" workflow focuses
on a single metric and simple, non-sliced charts. It has limitations with high-cardinality
axes, some growth/comparison syntax, and complex grouped formulas. ThoughtSpot also says
Spotter relies on metadata and documents constraints around data access, parameters,
chart configuration, and model selection depending on product version.[7][9]

Decision relevance:

- The exact headline use case is an established feature, not whitespace.
- A possible wedge is broader investigation across several raw tables and competing
  hypotheses, rather than single-chart change decomposition.
- That wedge must be tested against a disciplined baseline. It cannot be inferred from
  a longer agent trace or a more elaborate architecture.

## Where a measurable advantage could exist

No individual item below is novel. The combination could be valuable if it produces
better measured outcomes for the raw-data/no-semantic-layer user.

### A. Explicit falsification rather than only driver ranking

The agent should keep a typed register of candidate hypotheses and record, for each:

- why it was proposed;
- the exact query or computation used to test it;
- supporting and contradicting observations;
- whether it was rejected, retained, or left untestable;
- which missing data would discriminate between remaining explanations.

The measurable claim is not "shows its reasoning." It is higher root-cause recall with
fewer unsupported conclusions than the baseline.

### B. Evidence-preserving, rerunnable investigation bundles

Every reported number should link to a frozen input snapshot, executable SQL/Python,
parameters, and output checksum. Rerunning the bundle should reproduce reported tables
and charts within defined tolerances.

The measurable claim is provenance completeness and replay success, not merely a nice
report. This must be at least as inspectable as an Excel workbook or notebook.

### C. Honest separation of contribution, correlation, and causation

Change decomposition can identify which segment contributed to a metric movement; it
does not prove why the segment changed. The report should label:

- observed metric change;
- arithmetic contribution;
- statistical association;
- temporally aligned events;
- causal claims, which require an appropriate design or external evidence.

The measurable claim is a lower rate of causal overstatement on adversarial cases.

### D. Useful operation without a prepared semantic layer

DataDetective could ask a small number of high-information clarification questions,
infer provisional relationships, and expose those assumptions for confirmation. This
addresses users whom Power BI, Databricks, and ThoughtSpot expect to have curated models
or metadata.

The measurable claim is time-to-first-valid-investigation from raw exports, while
maintaining acceptable metric and join accuracy.

### E. Public, frozen evaluation rather than vendor feature claims

In the reviewed first-party product material, vendors document capabilities and
configuration guidance, but no comparable public evaluation was found for end-to-end
business-metric root-cause accuracy across these products. This is an opportunity for
credibility, not necessarily a product moat.

DataDetective should publish cases with known injected causes and score all systems on:

- true cause in top 1 and top 3 retained hypotheses;
- unsupported finding rate;
- metric-definition and join error rate;
- causal-overstatement rate;
- evidence-provenance completeness;
- full replay success;
- elapsed time, tool calls, and model cost;
- user clarification burden.

## What would only be cosmetic differentiation

The following claims should not be used to justify the project:

- "Chat with Excel/CSV using natural language."
- "The agent writes SQL or Python and draws charts."
- "The agent creates a plan before analyzing."
- "The agent explains why sales changed."
- "The agent produces a report with citations."
- "The system uses multiple agents to critique one another."
- "The system preserves a trace" without guaranteed rerun and evidence linkage.
- "The system uses RAG for metric definitions."

Each is already available in whole or in material part from the reviewed alternatives,
or is an implementation technique rather than a customer outcome.

## Recommended Wayfinder decision

Do not approve a full DataDetective product specification yet.

Proceed only to a viability protocol and a narrow investigation prototype with these
conditions:

1. The benchmark focuses on multi-table ecommerce metric anomalies with known causes.
2. The primary baseline is a strong general-purpose file-analysis assistant with the
   same files, business description, and token/tool budget.
3. A second conceptual baseline reflects established change-decomposition behavior,
   especially ThoughtSpot's documented why-question flow.
4. DataDetective's required advantage is measurable investigation reliability, not
   additional UI stages or longer traces.
5. Failure to improve top-3 cause recall and unsupported-finding rate should kill or
   materially reposition the project.

The most defensible positioning, if the benchmark succeeds, is:

> A reproducible anomaly-investigation agent for raw business exports that tests
> competing explanations and distinguishes what the data shows from what it cannot
> prove.

It should not be positioned as a general AI analyst or as the first agent that can
explain metric changes.

## Sources

1. OpenAI, [Data analysis with ChatGPT](https://help.openai.com/en/articles/8437071-data-analysis-with-chatgpt).
2. Microsoft, [Get started with Copilot in Excel](https://support.microsoft.com/en-us/office/get-started-with-copilot-in-excel-d7110502-0334-4b4f-a175-a73abdfc118a).
3. Microsoft, [Copilot for Power BI overview](https://learn.microsoft.com/en-us/power-bi/create-reports/copilot-introduction).
4. Microsoft, [Prepare your data for AI to improve Copilot results](https://learn.microsoft.com/en-us/power-bi/create-reports/copilot-prepare-data-ai).
5. Databricks, [Genie](https://docs.databricks.com/aws/en/genie/).
6. Databricks, [Create and manage a Genie Agent](https://docs.databricks.com/aws/en/genie/trusted-assets).
7. ThoughtSpot, [Why questions in Spotter](https://docs.thoughtspot.com/cloud/latest/spotter-why).
8. ThoughtSpot, [Spotter](https://docs.thoughtspot.com/cloud/latest/spotter).
9. ThoughtSpot, [Spotter limitations](https://docs.thoughtspot.com/cloud/latest/spotter-limitations).

