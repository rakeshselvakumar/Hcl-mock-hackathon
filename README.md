#  Insurance Policy Renewal & Claims Integration System 

## Project Overview

This project implements an end-to-end Insurance Analytics ETL pipeline using Informatica Intelligent Cloud Services (IICS), Oracle Database, Snowflake, and Power BI.

The system processes insurance data from multiple sources, validates records, performs transformations, routes valid and invalid records, loads analytics data into Snowflake, stores rejected records in Oracle, and visualizes insights through Power BI dashboards.

---

# Tech Stack

| Component | Technology |
|------------|------------|
| Source Database | Oracle SQL |
| ETL Tool | Informatica Intelligent Cloud Services (IICS) |
| Analytics Database | Snowflake |
| Reporting Tool | Power BI |
| Workflow | Parallel Taskflow |
| Notification | Email Notification |

---

# Architecture

```text
Oracle / CSV Sources
        ↓
IICS ETL Pipeline
        ↓
Validation + Transformation
        ↓
Router
      ↙      ↘
Valid      Invalid
 ↓            ↓
Snowflake    Oracle Reject Tables
 ↓
Power BI Dashboard
```

---

# Source Tables

## InsurancePolicy

| Column | Datatype |
|----------|-----------|
| POLICYID | INTEGER |
| POLICYHOLDERID | INTEGER |
| POLICYTYPE | VARCHAR(50) |
| COVERAGEAMOUNT | DECIMAL(12,2) |
| POLICYSTATUS | VARCHAR(20) |

---

## PolicyHolder

| Column | Datatype |
|----------|-----------|
| POLICYHOLDERID | INTEGER |
| POLICYHOLDERNAME | VARCHAR(100) |
| EMAIL | VARCHAR(100) |
| MOBILENUMBER | VARCHAR(20) |
| CUSTOMERSTATUS | VARCHAR(20) |

---

## Claim

| Column | Datatype |
|----------|-----------|
| CLAIMID | INTEGER |
| POLICYID | INTEGER |
| CLAIMAMOUNT | DECIMAL(12,2) |
| CLAIMDATE | DATE |
| CLAIMSTATUS | VARCHAR(20) |

---

## RenewalTransaction

| Column | Datatype |
|----------|-----------|
| RENEWALID | INTEGER |
| POLICYID | INTEGER |
| RENEWALAMOUNT | DECIMAL(12,2) |
| RENEWALDATE | DATE |
| PAYMENTSTATUS | VARCHAR(20) |

---

# Snowflake Target Tables

## INSURANCE_ANALYTICS

| Column | Datatype |
|----------|-----------|
| POLICYHOLDERID | INTEGER |
| POLICYHOLDERNAME | VARCHAR |
| EMAIL | VARCHAR |
| MOBILENUMBER | VARCHAR |
| CUSTOMERSTATUS | VARCHAR |
| CLAIMID | INTEGER |
| CLAIMAMOUNT | DECIMAL(12,2) |
| POLICYTYPE | VARCHAR |

---

## RENEWAL_ANALYTICS

| Column | Datatype |
|----------|-----------|
| RENEWALID | INTEGER |
| POLICYID | INTEGER |
| RENEWALAMOUNT | DECIMAL(12,2) |
| RENEWALDATE | DATE |
| PAYMENTSTATUS | VARCHAR |

---

# Oracle Reject Tables

## CLAIM_REJECT

| Column | Datatype |
|----------|-----------|
| REJECTID | INTEGER |
| CLAIMID | INTEGER |
| POLICYID | INTEGER |
| CLAIMAMOUNT | DECIMAL(12,2) |
| CLAIMDATE | DATE |
| CLAIMSTATUS | VARCHAR |
| REJECTREASON | VARCHAR(100) |
| REJECTEDDATE | DATE |

---

## RENEWAL_REJECT

| Column | Datatype |
|----------|-----------|
| REJECTID | INTEGER |
| RENEWALID | INTEGER |
| REJECTREASON | VARCHAR(100) |
| REJECTEDDATE | DATE |

---

# ETL Transformations Used

## Joiner Transformation
- Combines InsurancePolicy and Claim data
- Combines InsurancePolicy and Renewal data

## Lookup Transformation
- Validates records against existing data

## Expression Transformation
- Creates calculated fields
- Generates validation flags
- Generates reject reasons

Example:

```text
IIF(claimAmount>0,'VALID','INVALID')

TO_DECIMAL(claimAmount)
```

## Router Transformation
- Routes valid and invalid records

## Aggregator Transformation
- Summarizes records using COUNT and SUM operations

## Transaction Control
- Controls commit and rollback operations

Example:

```text
IIF(total_claims>0,
TC_COMMIT_BEFORE,
TC_ROLLBACK_BEFORE)
```

---

# Taskflow Implementation

## Parallel Taskflow

```text
Start
    ↓
Parallel Paths
↙      ↓      ↘
PolicyClaim
Renewal
Fraud
   ↘      ↓      ↙
Notification
      ↓
     End
```

Features:
- Parallel execution
- Conditional branching
- Email notification

---

# Power BI Dashboard

## Dashboard Pages

### Page 1 - Insurance Analytics
- Claim Amount by Policy Type
- Coverage Analysis
- Customer Status Distribution
- Total Claims Summary

### Page 2 - Renewal Analytics
- Renewal Trend Analysis
- Payment Status Distribution
- Renewal Revenue Summary

### Page 3 - Fraud / Reject Monitoring
- Reject Reason Analysis
- Rejected Claims Monitoring
- Fraud Summary

### Page 4 - Interactive Filtering
Slicers Used:
- Policy Type
- Claim Date
- Renewal Date

---

# Project Outcome

Successfully implemented:

✔ ETL workflow

✔ Data validation

✔ Reject handling

✔ Snowflake analytics storage

✔ Power BI dashboard

✔ Parallel taskflow

✔ Aggregator transformation

✔ Transaction control

✔ Email notification

---

# Author

Rakesh
