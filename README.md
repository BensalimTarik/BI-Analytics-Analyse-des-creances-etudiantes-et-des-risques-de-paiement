# BI-Analytics-Student-Receivables-Payment-Risk
# Student Receivables & Payment Risk Analytics | Power BI

## Project Overview

This project presents an interactive Power BI solution dedicated to the analysis of **student receivables** and **payment risk** (Credit & Collection Risk) across a network of **14 schools** offering several training programs.

It provides insights into outstanding balances, payment schedules, aging of receivables, collection performance, late-payment risk segmentation and data reconciliation between the billing system and the analytical model.

The solution combines a **dimensional data model**, **Power BI dashboards** and a **Python scoring module** that estimates the risk of late payment, so that collection teams can prioritize their actions.

## Dashboard Preview

The solution is organized into five analytical dashboards:

<table>
  <tr>
    <td align="center">
      <img src="Dashboard-Preview/1 - Aperçu financier de direction.jpg" width="100%" alt="Executive Financial Overview"/>
      <br/><b>1. Executive Financial Overview</b><br/><i>Aperçu financier de direction</i>
    </td>
    <td align="center">
      <img src="Dashboard-Preview/2 - Créances et échéancier.jpg" width="100%" alt="Receivables and Payment Schedule"/>
      <br/><b>2. Receivables and Payment Schedule</b><br/><i>Créances et échéancier</i>
    </td>
  </tr>
  <tr>
    <td align="center">
      <img src="Dashboard-Preview/3 - Performance de recouvrement.jpg" width="100%" alt="Collection Performance"/>
      <br/><b>3. Collection Performance</b><br/><i>Performance de recouvrement</i>
    </td>
    <td align="center">
      <img src="Dashboard-Preview/4 - Segmentation des risques d'impayé.jpg" width="100%" alt="Unpaid Risk Segmentation"/>
      <br/><b>4. Unpaid Risk Segmentation</b><br/><i>Segmentation des risques d'impayé</i>
    </td>
  </tr>
  <tr>
    <td align="center" colspan="2">
      <img src="Dashboard-Preview/5 - Rapprochement des données.jpg" width="50%" alt="Data Reconciliation"/>
      <br/><b>5. Data Reconciliation</b><br/><i>Rapprochement des données</i>
    </td>
  </tr>
</table>

## Key Figures

Figures as shown in the dashboards at the last data refresh (11 November 2026, 14:01). Student and invoice identifiers are anonymized (e.g. `STU000001`, `FAC0000001`).

### Financial overview

| Indicator | Value |
|---|---:|
| Total invoiced | 366.96 M€ |
| Total collected | 345.82 M€ |
| Total outstanding balance | 21.14 M€ |
| Overdue outstanding balance | 19.70 M€ |
| Overdue unpaid rate | 5.40 % |
| Overdue collection rate | 94.60 % |
| Open invoices | 17 K |
| Overdue invoices | 16 K |

### Receivables aging

| Indicator | Value |
|---|---:|
| Not yet due | 1.44 M€ |
| 0–30 days | 3.7 M€ |
| 31–60 days | 2.14 M€ |
| 61–90 days | 42.75 K€ |
| > 90 days | 13.79 M€ |
| Overdue share of outstanding balance | 93.19 % |
| Share of outstanding balance overdue by more than 60 days | 65.45 % |

### Collection performance

| Indicator | Value |
|---|---:|
| Total reminders sent | 200 K |
| Invoices with at least one reminder | 80 K |
| Amount collected after due date | 155.01 M€ |
| Average payment time | 24 days |
| Average payment delay | 28 days |
| Favorable reminder outcome rate | 54.93 % |
| Reminders per invoice | 2.50 |
| Invoices settled after a reminder | 80.25 % |

### Payment risk

| Indicator | Value |
|---|---:|
| Average outstanding balance per open invoice | 1.21 K€ |
| Unpaid rate by number of installments | 7.25 % |
| Outstanding concentration, top 5 schools | 57.01 % |
| Largest payer type by outstanding balance | Family (7.7 M€) |

### Data reconciliation

| Indicator | Value |
|---|---:|
| Invoices | 223 K |
| Payments | 222 K |
| Reminders | 200 K |
| Financial reconciliation gap | 3.42E-5 |
| Aging distribution gap | 1E-8 |
| Last refresh | 11/11/2026 at 14:01 |

## Main Analytical Areas

- **Executive Financial Overview:** consolidated view of invoiced amounts, collected amounts, outstanding balance and overdue unpaid rate for the whole network.
- **Receivables and Payment Schedule:** aging balance (0–30, 31–60, 61–90, > 90 days), due dates and upcoming installments by school and program.
- **Collection Performance:** collection rate, average payment delay and trends over time, by school, program and payment method.
- **Unpaid Risk Segmentation:** segmentation of students and receivables by late-payment risk level, based on the Python scoring module.
- **Data Reconciliation:** consistency checks between the billing system (Dynamics 365), the SQL Server data model and the dashboards (invoices, payments, balances, orphan records, duplicates).

## Interactive Filters

- School (14 schools)
- Program / training course
- Academic year and training period
- Invoice status and payment status
- Aging bucket (0–30, 31–60, 61–90, > 90 days)
- Risk level (low, medium, high)

## Architecture

```text
Dynamics 365 / billing software
            │
            ▼
      SQL Server (staging → analytical database)
            │
            ▼
   Dimensional model (facts + dimensions)
            │
     ┌──────┴───────┐
     ▼              ▼
 Power BI       Python scoring module
 (5 dashboards)  (late-payment risk)
```

## Technologies

- Microsoft Power BI Desktop
- Power Query
- DAX
- SQL Server (T-SQL)
- Python (Pandas, Scikit-learn)
- Microsoft Dynamics 365 / billing software (source data)
- Excel (dataset exchange format)

## Repository Structure

```text
BI-Analytics-Student-Receivables-Payment-Risk/
│
├── Dashboard-Preview/
│   ├── 1 - Executive Financial Overview.jpg
│   ├── 2 - Receivables and Payment Schedule.jpg
│   ├── 3 - Collection Performance.jpg
│   ├── 4 - Unpaid Risk Segmentation.jpg
│   └── 5 - Data Reconciliation.jpg
│
├── Specifications-Overview/
│   ├── README.md        (functional specification, French)
│   └── README_EN.md     (functional specification, English)
│
└── README.md
```

## Documentation

- [Functional specification (French)](Specifications-Overview/README.md)
- [Functional specification (English)](Specifications-Overview/README_EN.md)

---

**Project:** Student Receivables & Payment Risk Analytics (Credit & Collection Risk)  
**Tool:** Microsoft Power BI Desktop  
**Scope:** 14-school network, several training programs
