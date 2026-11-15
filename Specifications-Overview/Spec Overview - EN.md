# Functional Specifications – Student Receivables & Payment Risk Analysis

## 1. Project Overview

This project sets up a structured and sustainable framework to **monitor student receivables** and **analyze payment risk** (Credit & Collection Risk) for a network of **14 schools** offering several training programs.

It centralizes billing, payment schedule and collection data, measures outstanding balances and their aging, assesses collection performance and identifies students at risk of late payment. The results help the finance department and collection teams prioritize their actions and secure cash flow.

## 2. Business Objectives

The overall objective is to automate the processing chain, from the extraction of billing data to its delivery in decision-support dashboards.

The specific objectives are:

- **O1 – Visibility:** consolidate outstanding balances and unpaid amounts of the 14 schools into a single view.
- **O2 – Anticipation:** track the payment schedule and upcoming amounts to forecast collections.
- **O3 – Collection:** measure collection performance and the average payment delay.
- **O4 – Risk:** identify students and receivables at risk of late payment through a scoring model.
- **O5 – Reliability:** ensure consistency between the billing system, the analytical database and the dashboards.
- **O6 – Steering:** cover the priority analytical needs of executive management, school directors and collection teams.

## 3. Functional Scope and Process

The framework covers six main steps:

1. **Source data extraction:** retrieve invoices, payment schedules, payments, students and programs from Dynamics 365 / the billing software.
2. **Centralization:** load the data into a dedicated SQL Server database (staging area, then analytical database).
3. **Transformation and preparation:** cleaning, normalization, quality controls and KPI calculation.
4. **Modeling:** build a dimensional model (fact and dimension tables) suited to Power BI.
5. **Risk scoring:** a Python module computes a late-payment risk score per student or per receivable.
6. **Reporting:** create five interactive Power BI dashboards.

## 4. Dashboards

| No. | Dashboard | Purpose | Main content |
|---|---|---|---|
| 1 | Executive Financial Overview | Provide a consolidated view for the Group | Invoiced and collected amounts, total outstanding balance, overdue unpaid rate, monthly trend, school comparison |
| 2 | Receivables and Payment Schedule | Track aging and due dates | Aging balance (0–30, 31–60, 61–90, > 90 days), upcoming due dates, detail by school, program and status |
| 3 | Collection Performance | Measure collection effectiveness | Collection rate, average payment delay, trends, comparison by school, program and payment method |
| 4 | Unpaid Risk Segmentation | Prioritize follow-up actions | Risk levels (low, medium, high) from the Python score, segment characteristics, list of priority cases |
| 5 | Data Reconciliation | Ensure the reliability of the figures | Gaps between billing, analytical database and reports, orphan records, duplicates, data-entry anomalies |

## 5. Key Performance Indicators (KPIs)

| KPI | Definition / calculation rule | Target |
|---|---|---|
| Total invoiced | Sum of invoiced amounts over the period | Trend monitoring |
| Total collected | Sum of payments received over the period | Trend monitoring |
| Total outstanding balance | Σ (invoiced amount − collected amount) of unpaid invoices | Trend monitoring |
| Overdue unpaid rate | Overdue unpaid amounts / overdue amounts × 100 | To be defined (suggested: < 10%) |
| Aging balance | Overdue balance split by days past due: 0–30, 31–60, 61–90, > 90 days | Reduce the > 90 days bucket |
| Average payment delay | Average (payment date − due date) in days, weighted by amounts | To be defined (suggested: ≤ 15 days) |
| Collection rate | Collected amounts / overdue amounts × 100 | To be defined (suggested: ≥ 90%) |
| High-risk students | Number and share of students whose score exceeds the high-risk threshold | Trend monitoring |

Targets are indicative and must be validated and adjusted with the finance department.

## 6. Data Architecture and Sources

### Data Sources

| Source | Description | Access |
|---|---|---|
| Dynamics 365 / billing software | Invoices, payment schedules, payments, students, programs, schools | Read access per permissions |
| SQL Server database (staging) | Raw copy of source data | Controlled read and write |
| SQL Server Data Analytics database | Transformed data and dimensional model for Power BI | Controlled read and write |

Data is extracted from the source system and transferred to the analytical database at a frequency defined by project needs.

### Dimensional Model (proposal)

| Type | Table | Content |
|---|---|---|
| Fact | Fact_Invoices | Invoiced amount, invoice date, due date, status |
| Fact | Fact_Payments | Paid amount, payment date, payment method |
| Fact | Fact_Schedule | Scheduled installments, upcoming amounts, delays |
| Dimension | Dim_Student | Anonymized student ID, profile, status |
| Dimension | Dim_School | School, city, region |
| Dimension | Dim_Program | Program, level, duration, academic year |
| Dimension | Dim_Date | Calendar (day, month, quarter, year) |
| Dimension | Dim_Status | Invoice and payment statuses |
| Output | Risk_Score | Late-payment risk score, risk level, calculation date |

### Data Quality

Controls cover six dimensions:

- **Completeness:** presence of mandatory information (identifiers, amounts, due dates).
- **Accuracy:** validity of amounts and dates.
- **Consistency:** agreement between invoices, schedules and payments.
- **Uniqueness:** unique identification of invoices and payments, duplicate control.
- **Timeliness:** data refreshed at a documented frequency.
- **Traceability:** documentation of sources, permissions and processing.

## 7. Business Rules

| ID | Rule | Description | Status |
|---|---|---|---|
| RG-DEF-001 | Overdue receivable | A receivable is overdue when its due date has passed and its balance is greater than zero. | Draft |
| RG-CAL-001 | Aging balance | Aging is computed in days past due relative to the due date, using the 0–30, 31–60, 61–90 and > 90 days buckets. | Draft |
| RG-CAL-002 | Collection rate | Calculated on overdue amounts as of the report cut-off date. | Draft |
| RG-PER-001 | Scope | Selected schools, programs and periods are included; cancelled invoices and credit notes are handled separately. | Draft |
| RG-QUA-001 | Quality | Identifiers, amounts and dates are checked before loading into the analytical database. | Draft |
| RG-EXC-001 | Anomalies | Payments not linked to an invoice, duplicates and inconsistent amounts are flagged and verified before reintegration. | Draft |
| RG-SCO-001 | Scoring | The risk score is recalculated at a defined frequency; low, medium and high thresholds are validated by the finance department. | Draft |
| RG-SEC-001 | Security | Access to personal data and results is restricted by authorized roles and scope (row-level security by school). | Draft |

## 8. Project Value

The framework provides a reference set of indicators and interactive dashboards that make it possible to:

- Monitor outstanding balances, unpaid amounts and aging across the network.
- Compare schools, programs and cohorts.
- Anticipate collections thanks to the payment schedule.
- Measure collection performance and the average payment delay.
- Target follow-ups on the cases with the highest risk of late payment.
- Verify consistency between the billing system and the reports.
- Support financial steering and cash flow security.

---

**Project:** Student Receivables & Payment Risk Analysis  
**Scope:** 14-school network  
**Reporting tool:** Microsoft Power BI

