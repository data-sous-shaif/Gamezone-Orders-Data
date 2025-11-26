# 📚 Data Dictionary - Gamezone Orders

**Dataset:** orders_cleaned.csv  
**Last Updated:** 2025-11-06  
**Total Records:** 21,864  
**Valid Shipping Records:** 19,864 (90.5%)

---

## 📊 Dataset Overview

### Summary Statistics
- **Date Range:** 0 - 21864
- **Total Revenue:** $ 6151266.49
- **Unique Customers:** 19851
- **Unique Products:** 8
- **Countries:** 150 

### Data Quality Notes
⚠️ **Invalid Ship Dates:** 2,000 orders (9.15%) have ship_ts before purchase_ts and 2 orders where ship_ts are > 300+ days from the time of purchase due to data corruption. These records are:
- ✅ **INCLUDED** in revenue, customer, product, and marketing analyses
- ❌ **EXCLUDED** from operational shipping metrics

---

## 📋 Column Definitions

### Core Identifiers

| Column | Type | Description | Example | Nullable |
|--------|------|-------------|---------|----------|
| **order_id** | string | Order reference |  | Yes |
| **user_id** | string | User account ID | USER_54321 | NO |

### Transaction Details

| Column | Type | Description | Example | Nullable |
|--------|------|-------------|---------|----------|
**purchase_ts** | datetime | Order purchase timestamp | 2024-01-15 14:23:00 | No |
| **purchase_ts_cleaned** | datetime | Order purchase timestamp | 2024-01-15 14:23:00 | No |
| **ship_ts** | datetime | Order ship timestamp (see quality note) | 2024-01-17 09:15:00 | Yes |
| **revenue** | float | Order value in USD | 299.99 | No |
| **product_name** | string | Product purchased | 27in 4K Gaming Monitor | No |
| **product_name_cleaned** | string | Standardized product name | 27in 4k gaming monitor | No |

### Geographic

| Column | Type | Description | Example | Nullable |
|--------|------|-------------|---------|----------|
| **country_code** | string | ISO country code (uppercase) | US | No |
| **region** | string | Geographic region | NA/EMEA | No |

### Marketing

| Column | Type | Description | Example | Nullable |
|--------|------|-------------|---------|----------|
| **marketing_channel** | string | Acquisition channel | Email, Social, Direct | No |
| **marketing_channel_cleaned** | string | Acquisition channel | Email, Social, Direct | No |
| **purchase_platform** | string | Purchase platform | Website, Mobile App | No |

### Derived Fields (Created During Cleaning)

| Column | Type | Description | Calculation | Nullable |
|--------|------|-------------|-------------|----------|
| **purchase_year** | int | Year of purchase | Extracted from purchase_ts | No |
| **purchase_month** | int | Month of purchase (1-12) | Extracted from purchase_ts | No |
| **time_to_ship** | int | Days from purchase to ship | (ship_ts - purchase_ts).days | Yes |
| **date_Check** | bool | Ship date before purchase | ship_ts < purchase_ts | No |

---

## 🚨 Known Data Quality Issues

### Issue #1: User ID Scientific Notation
- **Status:** ✅ Resolved
- **Impact:** 36 rows (0.16%)
- **Resolution:** Converted to text format

### Issue #2: Inconsistent Date Formats
- **Status:** ✅ Resolved
- **Impact:** 10 rows (0.05%)
- **Resolution:** Parsed using pd.to_datetime with dayfirst=True

### Issue #3: Incomplete Data
- **Status:** ✅ Resolved
- **Impact:** 1 row (0.00%)
- **Resolution:** Negligible impact, kept in dataset

### Issue #4: Invalid Ship Dates (CRITICAL)
- **Status:** ⚠️ Unresolvable - Flagged
- **Impact:** 2,000 rows (9.15%)
- **Root Cause:** Data corruption (ship dates 1-247 days before purchase)
- **Resolution:** 
  - Included in revenue/customer analyses
  - Excluded from operational analyses
- **Documentation:** See `Data_Validation.ipynb`, Issue #4

---


