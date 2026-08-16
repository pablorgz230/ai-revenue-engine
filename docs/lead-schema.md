# Lead Data Schema

## 1. Purpose

Define the structure, fields, data types and validation rules for
leads used by the CedresOps lead scoring system.

The schema will be used as the contract between the lead dataset,
the scoring engine and future AI-based systems.

---

## 2. Lead Structure

Each lead represents a company that may potentially become a
CedresOps customer.

Each lead must contain a unique identifier and the information
required to evaluate its ICP fit and commercial potential.

---

## 3. Lead Fields

| Field | Type | Required | Description |
|---|---|---|---|
| lead_id | string | Yes | Unique identifier for the lead |
| company_name | string | Yes | Company name |
| business_model | string | Yes | Company's primary business model |
| industry | string | Yes | Primary industry |
| country | string | Yes | Company's primary country |
| geography | string | Yes | Primary operating market |
| employees | integer | Yes | Current number of employees |
| employee_growth_12m | float | Yes | Employee growth over the previous 12 months (%) |
| sales_headcount | integer | Yes | Current number of sales employees |
| sales_headcount_growth_6m | float | Yes | Sales headcount growth over the previous 6 months (%) |
| open_sales_roles | integer | Yes | Number of currently open sales positions |
| funding_last_24m_eur | float | Yes | Funding raised during the previous 24 months (€) |
| new_markets_last_12m | integer | Yes | Number of new European markets entered during the previous 12 months |
| lead_volume_growth_12m | float | Yes | Lead volume growth over the previous 12 months (%) |
| crm | string | Yes | Primary CRM platform |
| recurring_revenue | boolean | Yes | Whether the company has a recurring revenue model |
| revops_team | boolean | Yes | Whether the company has a dedicated RevOps function |
| revops_headcount | integer | No | Number of employees dedicated to RevOps |

---

## 4. Field Definitions

### 4.1 lead_id

Unique identifier assigned to each lead.

Format:

`L-0001`

Example:

`L-0001`

---

### 4.2 company_name

Name of the company represented by the lead.

Example:

`Example SaaS`

---

### 4.3 business_model

Primary business model of the company.

Initial supported value:

`B2B SaaS`

The field may later support additional business models for
testing hard ICP filters.

---

### 4.4 industry

Primary industry or sector in which the company operates.

Examples:

- B2B SaaS
- FinTech
- HealthTech
- MarTech
- Cybersecurity

---

### 4.5 country

Primary country of the company.

Examples:

- Spain
- Germany
- France
- Netherlands
- Sweden

---

### 4.6 geography

Primary geographic market in which the company operates.

For the initial dataset:

`Europe`

---

### 4.7 employees

Current total number of employees.

Type:

`integer`

Example:

`250`

---

### 4.8 employee_growth_12m

Percentage change in total employee count over the previous
12 months.

Type:

`float`

Example:

`18.5`

This represents:

`18.5%`

---

### 4.9 sales_headcount

Current number of employees primarily working in sales.

Type:

`integer`

Example:

`35`

---

### 4.10 sales_headcount_growth_6m

Percentage change in sales headcount over the previous
6 months.

Type:

`float`

Example:

`22.0`

This represents:

`22%`

---

### 4.11 open_sales_roles

Number of currently active sales-related job openings.

Type:

`integer`

Example:

`7`

---

### 4.12 funding_last_24m_eur

Total funding raised during the previous 24 months.

Type:

`float`

Unit:

EUR

Example:

`12000000`

This represents:

`€12,000,000`

---

### 4.13 new_markets_last_12m

Number of new European markets entered during the previous
12 months.

Type:

`integer`

Example:

`2`

---

### 4.14 lead_volume_growth_12m

Percentage change in generated lead volume over the previous
12 months.

Type:

`float`

Example:

`35.0`

This represents:

`35%`

---

### 4.15 crm

Primary CRM platform used by the company.

Initial supported values:

- Salesforce
- HubSpot
- Microsoft Dynamics
- Other
- None

---

### 4.16 recurring_revenue

Indicates whether the company operates using a recurring
revenue model.

Type:

`boolean`

Possible values:

- `true`
- `false`

This field is also used as a hard ICP filter.

---

### 4.17 revops_team

Indicates whether the company has a dedicated Revenue Operations
function.

Type:

`boolean`

Possible values:

- `true`
- `false`

---

### 4.18 revops_headcount

Number of employees dedicated to Revenue Operations.

Type:

`integer`

This field is optional because some companies may have a RevOps
function without publicly available headcount information.

Example:

`4`

---

## 5. Validation Rules

The following validation rules apply to the initial schema.

### lead_id

Must be unique.

Format:

`L-XXXX`

---

### employees

Must be a positive integer.

Expected range:

`> 0`

---

### employee_growth_12m

Must be a numeric percentage.

Expected range:

`>= -100`

---

### sales_headcount

Must be a positive integer or zero.

Expected range:

`>= 0`

---

### sales_headcount_growth_6m

Must be a numeric percentage.

Expected range:

`>= -100`

---

### open_sales_roles

Must be a non-negative integer.

Expected range:

`>= 0`

---

### funding_last_24m_eur

Must be a non-negative number.

Expected range:

`>= 0`

---

### new_markets_last_12m

Must be a non-negative integer.

Expected range:

`>= 0`

---

### lead_volume_growth_12m

Must be a numeric percentage.

Expected range:

`>= -100`

---

### revops_headcount

If provided, must be a non-negative integer.

Expected range:

`>= 0`

---

## 6. Example Lead

Example lead object:

    {
      "lead_id": "L-0001",
      "company_name": "NordicCloud",
      "business_model": "B2B SaaS",
      "industry": "Cybersecurity",
      "country": "Sweden",
      "geography": "Europe",
      "employees": 250,
      "employee_growth_12m": 18.5,
      "sales_headcount": 35,
      "sales_headcount_growth_6m": 22.0,
      "open_sales_roles": 7,
      "funding_last_24m_eur": 12000000,
      "new_markets_last_12m": 2,
      "lead_volume_growth_12m": 35.0,
      "crm": "Salesforce",
      "recurring_revenue": true,
      "revops_team": true,
      "revops_headcount": 4
    }

---

## 7. Data Representation

The initial synthetic dataset will be stored in JSON format.

Future versions may additionally support:

- CSV
- PostgreSQL
- HubSpot
- API responses
- n8n workflow data
- Structured LLM outputs

The JSON representation will serve as the initial canonical
representation of a CedresOps lead.

---

## 8. Schema Evolution

Changes to the lead schema must be documented before being
implemented.

Schema changes may include:

- Adding new fields
- Removing fields
- Changing field types
- Changing validation rules
- Adding new enumerated values

Major schema changes should be documented in:

`docs/decisions.md`

The schema should remain backwards-compatible where practical.