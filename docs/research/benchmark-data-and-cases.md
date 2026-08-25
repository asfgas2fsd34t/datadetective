# DataDetective Benchmark Data and Case Research

Research date: 2026-08-25

## Decision

No reviewed public dataset is simultaneously:

1. a sufficiently rich e-commerce business dataset,
2. licensed for unrestricted reuse in a hosted or commercial product, and
3. labeled with the business causes of metric anomalies.

The benchmark should therefore use a **deterministic synthetic e-commerce generator with hidden intervention manifests as its scored core**. A real-data suite can test ecological validity, but it must not be treated as ground-truth root-cause data.

Recommended tiers:

| Tier | Purpose | Data | Distribution |
| --- | --- | --- | --- |
| Core | Scored regression and public leaderboard | Project-owned synthetic relational data plus deterministic interventions | Release under the project's license |
| Real-data holdout | Check behavior on real transaction distributions | UCI Online Retail, with attribution and exact upstream version/hash | May be redistributed under CC BY 4.0 |
| Research-only holdout | Rich fulfillment, review, funnel, and clickstream cases | Olist and optionally Retailrocket | Keep separate; CC BY-NC-SA 4.0 prevents commercial use |
| Access-only check | Realistic event schema and ad hoc demonstrations | Google GA4 sample e-commerce dataset in BigQuery | Query in place; do not bundle absent a confirmed data license |

This is a product-planning recommendation, not legal advice. License obligations should be reviewed before distributing any derived dataset.

## Candidate datasets

### 1. UCI Online Retail: approved real-data base

The UCI repository describes **541,909 transactions** from a UK non-store retailer between 2010-12-01 and 2011-12-09. Fields include invoice, product, quantity, timestamp, unit price, customer, and country; an invoice beginning with `C` denotes a cancellation. The repository assigns DOI `10.24432/C5BW33` and licenses the dataset under **CC BY 4.0**.[^uci-retail-api][^uci-retail-page]

CC BY 4.0 permits sharing and adaptation for any purpose, including commercially, provided attribution and the other license conditions are met.[^cc-by]

**Useful for:** revenue/quantity changes, average order value, product or country mix shifts, repeat-customer behavior, and cancellation-rate cases.

**Not sufficient for:** traffic-to-purchase conversion, checkout funnels, fulfillment, advertising, refunds as distinct from cancellation, or a true causal label. It is one retailer and many customers are wholesalers, so it is not representative of all consumer e-commerce.

The two-year **Online Retail II** dataset contains 1,067,371 transactions from 2009-12-01 through 2011-12-09 and is also CC BY 4.0.[^uci-retail-ii] It overlaps Online Retail, so the two must not be split across train and test as if they were independent. Use Online Retail II only if a second prior-year season is essential; otherwise prefer the smaller CSV-backed Online Retail release.

**Verdict:** include as the default real-data holdout, not as the scored causal ground truth.

### 2. Olist Brazilian E-Commerce plus Marketing Funnel: approved only for noncommercial research

Olist's first-party Kaggle listing describes approximately **100,000 anonymized real orders from 2016 to 2018**, with order status, price, payments, freight performance, customer location, product attributes, sellers, and reviews. The listing identifies Olist as owner and marks the dataset **CC BY-NC-SA 4.0**.[^olist-orders]

Olist separately publishes a randomly sampled marketing-funnel dataset of roughly **8,000 marketing-qualified leads**. It can join to the order data by `seller_id` and includes the seller acquisition process from landing-page signup through won deal. It carries the same CC BY-NC-SA 4.0 license.[^olist-funnel]

CC BY-NC-SA 4.0 allows sharing and adaptation but prohibits commercial use and requires adaptations to use the same license.[^cc-by-nc-sa]

**Useful for:** delivery-delay concentration, seller/category/region effects, review-score deterioration, payment mix, order cancellation, lead-source mix, and lead-to-seller conversion.

**Limitation:** the data records outcomes, not documented causal interventions. A late-delivery/review relationship found in the records is evidence of association, not a known root cause. Hidden deterministic interventions are still required for scored cause recall.

**Verdict:** excellent research-only ecological suite. Do not make it a dependency of a paid hosted product or combine it silently into a permissively licensed benchmark bundle.

### 3. Retailrocket event data: optional noncommercial funnel holdout

The Retailrocket Kaggle listing describes 4.5 months of anonymized behavior from a real e-commerce site: 2,756,101 events consisting of views, add-to-cart events, and transactions from 1,407,580 visitors, plus time-varying item properties and a category tree. The listing marks the files **CC BY-NC-SA 4.0**.[^retailrocket]

**Useful for:** traffic, browse-to-cart, cart-to-purchase, product/category funnel, and behavioral mix cases that UCI cannot represent.

**Limitations:** noncommercial/share-alike restrictions, hashed semantics, short observation window, and no cause labels. The Kaggle owner metadata is less clearly first-party than Olist's, so provenance should be confirmed before redistribution.

**Verdict:** optional research-only suite; not part of the core release.

### 4. Microsoft WideWorldImporters: permissive synthetic supplement

Microsoft describes WideWorldImporters as a fictional wholesale company with OLTP and OLAP sample databases, workload drivers, and analytics examples. The official `microsoft/sql-server-samples` repository is MIT licensed.[^wwi-readme][^wwi-license]

**Useful for:** testing relational joins, warehouse-style schemas, order/invoice analysis, and repeatable workload generation under a permissive license.

**Limitations:** wholesale rather than consumer e-commerce, SQL Server-oriented, and not labeled for anomaly causes.

**Verdict:** useful as an engineering compatibility fixture, but weaker than a purpose-built project-owned e-commerce generator for product evaluation.

### 5. Google GA4 sample e-commerce data: access-only

Google provides three months of obfuscated Google Merchandise Store event exports in BigQuery, covering 2020-11-01 through 2021-01-31. Google warns that the data only emulates a real-world dataset, contains placeholder values, has limited internal consistency, and requires a Google Cloud project with BigQuery enabled.[^ga4-sample]

The official page links Google Analytics terms but does not state a dataset-specific redistribution license for the data itself. Public query access must not be interpreted as permission to redistribute a frozen copy.

**Verdict:** useful for an optional connector demonstration or access-in-place compatibility test. Exclude from the downloadable benchmark until redistribution rights are confirmed.

### Rejected or secondary copies

- Kaggle's `carrie1/ecommerce-data` is a secondary copy of UCI Online Retail and reports its license as `Unknown`; use the DOI-backed UCI source instead.[^kaggle-uci-copy]
- REES46's large multi-category behavior listing reports `Data files (c) Original Authors`, not a reusable standard license; public download alone is insufficient for redistribution.[^rees46]
- dbt Labs' Jaffle Shop is attractive generated commerce-like data, but the inspected official repository did not contain a license file. Without an explicit license, it should not be copied into the benchmark.

## Why natural anomalies are not enough

The public e-commerce datasets above expose observations, not intervention logs. They can show that revenue, conversion, delivery, or reviews changed and which segments co-moved, but they generally cannot establish why.

This distinction matters because the proposed product evaluates **investigation**, not merely anomaly detection. A benchmark that labels the lowest segment as the "root cause" would reward plausible correlation and bake unsupported causal claims into the answer key.

Academic anomaly benchmarks reinforce the useful parts of the benchmark design but do not solve the e-commerce root-cause gap:

- Numenta's NAB contains more than 50 labeled real and artificial time series and records anomaly windows. Its `realKnownCause` subset has externally documented events such as holidays, a snowstorm, a planned shutdown, or system failure.[^nab-readme][^nab-data]
- GutenTAG, published by the TimeEval authors under MIT, generates seeded univariate or multivariate time series with exact anomaly positions and types such as mean, trend, variance, pattern, and mode-correlation. Its output includes an `is_anomaly` label and an overview manifest.[^gutentag-readme][^gutentag-types][^gutentag-usage]

NAB demonstrates the value of documented causes; GutenTAG demonstrates deterministic, seeded anomaly injection. Neither supplies a relational e-commerce investigation benchmark. DataDetective should adopt their reproducibility pattern while injecting **business mechanisms into rows and events**, not merely perturbing an already aggregated metric.

## Recommended frozen benchmark design

Each case should ship as an immutable directory:

```text
case-id/
  public/
    question.md
    metric-contract.yaml
    data/*.parquet
    data-dictionary.md
  private/
    intervention-manifest.yaml
    expected-causes.yaml
    evidence-assertions.sql
    unsupported-claim-rules.yaml
  provenance.yaml
```

`provenance.yaml` should record the generator commit, random seed, source dataset DOI/version where applicable, transformation commit, license, attribution text, and SHA-256 of every input and output artifact.

The hidden intervention manifest is the source of truth. It should specify:

- affected entity, segment, and time window;
- changed mechanism, such as session arrival, checkout completion, unit price, product mix, cancellation probability, or delivery duration;
- intervention size and expected metric contribution;
- unaffected controls and planted distractors;
- whether the evidence supports causation, association only, or no conclusion;
- acceptable aliases and granularity for the cause.

Synthetic interventions must be applied at the lowest meaningful grain, for example changing checkout outcomes on sessions rather than multiplying the final conversion-rate column. This keeps SQL evidence reproducible and lets the Agent discover the mechanism through joins and decomposition.

## Case catalog

The first frozen suite should contain at least these case families. Each family needs several seeds, effect sizes, and distractor configurations.

| Case family | Hidden mechanism | Expected investigation behavior | Suitable data tier |
| --- | --- | --- | --- |
| Traffic decline | Remove or suppress sessions from one channel/device/region | Separate traffic loss from conversion and order-value effects | Synthetic; Retailrocket/GA4 for realism |
| Checkout conversion decline | Lower purchase completion after cart for a segment | Trace funnel stages and localize the failing segment | Synthetic; Retailrocket for realism |
| Average-order-value decline | Shift demand toward cheaper products without changing within-product prices | Identify composition shift rather than claim a price cut | Synthetic; UCI/Olist holdout |
| True price/discount effect | Reduce unit prices for specified products or campaign orders | Quantify price effect separately from volume and mix | Synthetic; UCI/Olist holdout |
| Cancellation spike | Increase cancellation probability for a product/country/payment cohort | Distinguish gross orders from net completed orders | Synthetic; UCI/Olist holdout |
| Delivery SLA degradation | Add fulfillment or carrier delay for selected sellers/regions | Localize delay stage and affected cohort | Synthetic; Olist research-only |
| Review-score decline | Degrade reviews through an injected late-delivery mechanism | Find evidence chain while avoiding unsupported generalization | Synthetic; Olist research-only |
| Seller-acquisition conversion decline | Change lead mix or stage conversion for one source/segment | Separate volume, mix, and within-segment conversion | Synthetic; Olist funnel research-only |
| Simpson's-paradox mix shift | Change segment proportions while within-segment rates remain stable | Report aggregate reversal and segment-level truth | Synthetic |
| Data-quality incident | Duplicate orders, break join keys, shift timezone, or truncate a partition | Detect data defect before inventing a business cause | Synthetic plus transformed real holdout |
| Multi-cause incident | Inject two independent mechanisms with unequal contributions | Recover both causes and rank their contributions | Synthetic |
| Insufficient-evidence control | Show an anomaly but omit the table needed to distinguish causes | Explicitly request missing data and avoid a false root cause | Synthetic |
| No-anomaly control | Present normal seasonal variation or a metric-definition change | Reject the premise or clarify the metric | Synthetic plus UCI holdout |

The suite should balance obvious single-cause cases with gradual changes, delayed effects, multiple causes, confounding, data defects, and cases where the correct answer is "not identifiable from these data."

## Scoring implications

The data supports the following objective checks:

- **Hypothesis recall:** a manifest cause appears in the Agent's ranked top `k`, with segment and time window at an accepted granularity.
- **Evidence correctness:** cited values equal the results of frozen SQL assertions within declared tolerance.
- **Contribution accuracy:** estimated contribution to the metric delta is within a case-specific tolerance and does not double-count overlapping segments.
- **Reproducibility:** emitted SQL/Python runs against the frozen artifact and regenerates each cited table or number.
- **Unsupported-claim rate:** causal claims not licensed by the manifest/evidence are penalized; association-only cases must be qualified.
- **Data-quality precedence:** cases with corrupted inputs require the Agent to surface the defect before business diagnosis.
- **Honest abstention:** insufficient-evidence cases reward asking for the missing table or declaring non-identifiability.

Real-data holdouts without injected interventions can score executable evidence, arithmetic, citations, and unsupported claims. They **cannot fairly score root-cause recall** unless a hidden intervention was applied or an independent first-party incident record documents the cause.

## Practical recommendation for the Wayfinder

Proceed with viability testing only if the project is willing to build and publish the synthetic generator and its manifests as part of the benchmark. Do not base the go/no-go test on cherry-picked natural patterns in Olist or UCI.

For the first viability protocol:

1. Build 20-30 synthetic cases across at least eight families, including multi-cause, data-quality, and insufficient-evidence controls.
2. Keep case-generation code separate from the Agent and hide manifests during execution.
3. Add 5-10 UCI-derived holdouts to test realistic transaction distributions without claiming natural root-cause labels.
4. Run the same frozen cases against a general-purpose model plus notebook baseline.
5. Measure top-3 cause recall only on intervention-labeled cases; measure evidence correctness, reproducibility, and unsupported claims on both tiers.
6. Treat Olist and Retailrocket as optional, separately downloaded noncommercial research packs rather than a dependency of the public product.

This design gives the evaluation a legally defensible core, known causes, deterministic reruns, and realistic holdouts without confusing observed correlation with causal ground truth.

## Sources

[^uci-retail-api]: UCI Machine Learning Repository, [Online Retail API metadata](https://archive.ics.uci.edu/api/dataset?id=352), including size, period, DOI, and variable definitions.
[^uci-retail-page]: UCI Machine Learning Repository, [Online Retail dataset page](https://archive.ics.uci.edu/dataset/352/online+retail), including the CC BY 4.0 license notice.
[^cc-by]: Creative Commons, [Attribution 4.0 International deed](https://creativecommons.org/licenses/by/4.0/deed.en).
[^uci-retail-ii]: UCI Machine Learning Repository, [Online Retail II](https://archive.ics.uci.edu/dataset/502/online+retail+ii) and [API metadata](https://archive.ics.uci.edu/api/dataset?id=502).
[^olist-orders]: Olist/Kaggle, [Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) and [Kaggle dataset metadata API](https://www.kaggle.com/api/v1/datasets/view/olistbr/brazilian-ecommerce).
[^olist-funnel]: Olist/Kaggle, [Marketing Funnel by Olist](https://www.kaggle.com/datasets/olistbr/marketing-funnel-olist) and [Kaggle dataset metadata API](https://www.kaggle.com/api/v1/datasets/view/olistbr/marketing-funnel-olist).
[^cc-by-nc-sa]: Creative Commons, [Attribution-NonCommercial-ShareAlike 4.0 International deed](https://creativecommons.org/licenses/by-nc-sa/4.0/deed.en).
[^retailrocket]: Retailrocket/Kaggle, [Retailrocket recommender system dataset](https://www.kaggle.com/datasets/retailrocket/ecommerce-dataset) and [Kaggle dataset metadata API](https://www.kaggle.com/api/v1/datasets/view/retailrocket/ecommerce-dataset).
[^wwi-readme]: Microsoft, [WideWorldImporters official README](https://github.com/microsoft/sql-server-samples/blob/master/samples/databases/wide-world-importers/README.md).
[^wwi-license]: Microsoft, [`sql-server-samples` MIT license](https://github.com/microsoft/sql-server-samples/blob/master/license.txt).
[^ga4-sample]: Google for Developers, [BigQuery sample dataset for Google Analytics ecommerce web implementation](https://developers.google.com/analytics/bigquery/web-ecommerce-demo-dataset).
[^kaggle-uci-copy]: Kaggle, [secondary E-Commerce Data copy](https://www.kaggle.com/api/v1/datasets/view/carrie1/ecommerce-data), whose metadata reports `Unknown` license.
[^rees46]: Kaggle/REES46, [eCommerce behavior data from multi-category store](https://www.kaggle.com/api/v1/datasets/view/mkechinov/ecommerce-behavior-data-from-multi-category-store), whose metadata reports `Data files (c) Original Authors`.
[^nab-readme]: Numenta, [Numenta Anomaly Benchmark official repository](https://github.com/numenta/NAB), including its MIT license and benchmark description.
[^nab-data]: Numenta, [NAB data corpus documentation](https://github.com/numenta/NAB/blob/master/data/README.md), including the `realKnownCause` descriptions.
[^gutentag-readme]: TimeEval authors, [GutenTAG official repository](https://github.com/TimeEval/gutentag), including its MIT license and TimeEval paper citation (`10.14778/3554821.3554873`).
[^gutentag-types]: TimeEval authors, [GutenTAG anomaly types](https://github.com/TimeEval/gutentag/blob/main/doc/introduction/anomaly-types.md).
[^gutentag-usage]: TimeEval authors, [GutenTAG seeded generation, manifest, and output documentation](https://github.com/TimeEval/gutentag/blob/main/doc/usage.md).
