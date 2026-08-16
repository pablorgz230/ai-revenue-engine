# Lead Scoring Specification

## 1. Purpose

Define the rules, variables and expected outputs used to evaluate
the commercial potential of CedresOps leads.

The initial scoring system will be implemented as a deterministic
rule-based baseline.

A later version will compare this baseline against an LLM-based
scoring system.

---

## 2. Scoring Architecture

The scoring pipeline consists of four stages:

1. Data validation
2. Hard ICP filtering
3. Commercial signal evaluation
4. Lead score generation

```text
Raw Lead
   ↓
Data Validation
   ↓
Hard ICP Filters
   ↓
Signal Evaluation
   ↓
Lead Score
   ↓
Priority Classification

## 3. Hard ICP Filters

Hard filters determine whether a lead is fundamentally compatible
with the CedresOps ICP.

| Variable | Condition | Result if failed |
|---|---|---|
| business_model | B2B SaaS | Disqualified |
| geography | Europe | Disqualified |
| employees | >= 50 | Disqualified |
| employees | <= 1,000 | Disqualified |
| recurring_revenue | true | Disqualified |

A lead that fails any hard filter is classified as:

`disqualified`

Hard-filter failures should not be compensated by positive
commercial signals.

---

## 4. Scoring Variables

### 4.1 Employee Growth

Variable:

`employee_growth_12m`

Definition:

Percentage change in total employees over the previous 12 months.

| Value | Signal |
|---|---|
| < 0% | Negative |
| 0–5% | Neutral |
| 5–15% | Moderate |
| 15–30% | Strong |
| > 30% | Very Strong |

---

### 4.2 Sales Headcount

Variable:

`sales_headcount`

Definition:

Number of employees primarily working in sales.

Target:

`>= 10`

---

### 4.3 Sales Headcount Growth

Variable:

`sales_headcount_growth_6m`

Definition:

Percentage change in sales headcount over the previous 6 months.

| Value | Signal |
|---|---|
| < 0% | Negative |
| 0–10% | Neutral |
| 10–25% | Positive |
| > 25% | Strong |

---

### 4.4 Open Sales Roles

Variable:

`open_sales_roles`

Definition:

Number of currently active sales-related job openings.

| Value | Signal |
|---|---|
| 0 | Neutral |
| 1–2 | Moderate |
| 3–5 | Positive |
| > 5 | Strong |

---

### 4.5 Recent Funding

Variable:

`funding_last_24m_eur`

Definition:

Total funding raised during the previous 24 months.

| Value | Signal |
|---|---|
| €0 | Neutral |
| < €1M | Weak |
| €1M–€5M | Moderate |
| €5M–€20M | Strong |
| > €20M | Very Strong |

---

### 4.6 European Expansion

Variable:

`new_markets_last_12m`

Definition:

Number of new European markets entered during the previous
12 months.

| Value | Signal |
|---|---|
| 0 | Neutral |
| 1 | Positive |
| >= 2 | Strong |

---

### 4.7 Lead Volume Growth

Variable:

`lead_volume_growth_12m`

Definition:

Percentage change in generated leads over the previous 12 months.

| Value | Signal |
|---|---|
| < 0% | Negative |
| 0–10% | Neutral |
| 10–30% | Positive |
| > 30% | Strong |

---

### 4.8 CRM

Variable:

`crm`

Possible values:

- Salesforce
- HubSpot
- Microsoft Dynamics
- Other
- None

CRM presence is considered a positive indicator of commercial
maturity.

---

### 4.9 Revenue Operations Function

Variable:

`revops_team`

Possible values:

- true
- false

Optional additional variable:

`revops_headcount`

This variable will initially be treated as a signal rather than
a hard filter.

---

## 5. Priority Classification

The scoring system will produce a numerical score between:

`0–100`

Initial classification:

| Score | Classification |
|---|---|
| 0–39 | Low Priority |
| 40–69 | Medium Priority |
| 70–100 | High Priority |

Disqualified leads are not assigned a commercial priority.

---

## 6. Output Schema

The scoring system should produce structured JSON.

Example:

```json
{
  "lead_id": "L-0001",
  "score": 82,
  "classification": "high_priority",
  "hard_filter_status": "passed",
  "signals": [],
  "negative_signals": [],
  "recommended_action": "sales_outreach"
}

## 7. Baseline vs AI Scoring

The initial scoring system will be deterministic and rule-based.

This system will serve as the baseline against which an LLM-based
scoring system will later be evaluated.

The project will compare:

- Rule-based scoring
- LLM-based scoring
- Agreement between systems
- Classification consistency
- Evaluation performance

---

## 8. Scoring Weights

Scoring weights will not be defined at this stage.

Weights will be established after the synthetic dataset and
ground-truth labels have been created.

This prevents arbitrary weights from being introduced before
there is a defined evaluation dataset.

The final scoring model should document the rationale behind
each weighting decision.

---

## 9. Ground Truth

A labelled evaluation dataset will be created to establish the
expected commercial outcome for each synthetic lead.

Each lead will receive a ground-truth classification based on
the defined ICP and commercial criteria.

The ground truth will be used to evaluate both:

- The deterministic rule-based scoring system
- The LLM-based scoring system

The ground truth should be defined independently from the LLM
being evaluated.

---

## 10. Evaluation

The scoring systems will eventually be evaluated using metrics
appropriate for lead prioritization.

Initial evaluation metrics will include:

- Accuracy
- Precision
- Recall
- F1 score
- Confusion matrix
- Agreement between rule-based and LLM scoring

Additional business-oriented metrics may be introduced later.

Examples include:

- High-priority lead capture rate
- False-positive rate
- False-negative rate
- Top-N lead precision
- Commercial prioritization consistency

---

## 11. Future Extensions

Potential future improvements include:

- Dynamic scoring weights
- Model-based scoring
- Multi-model comparison
- Confidence scores
- Explainable scoring
- External company signals
- Real-time enrichment
- Historical outcome-based scoring
- Model performance monitoring
- Automated score recalculation

These features are outside the initial MVP scope.