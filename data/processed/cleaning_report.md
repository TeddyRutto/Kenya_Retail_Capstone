# Data Cleaning Report

Raw rows: transactions 167,293, customers 3,015.  
Clean rows: transactions 166,461, customers 3,000.

| Table | Issue | Before | After | Action |
|---|---|---:|---:|---|
| customers | Duplicate rows | 15 | 0 | Dropped exact duplicates |
| customers | County not matching a store county (case/spaces) | 119 | 0 | Stripped whitespace and applied title case |
| customers | Missing segment | 30 | 0 | Filled with 'Unknown' (not guessed) |
| transactions | Duplicate rows | 832 | 0 | Dropped exact duplicates |
| transactions | Dates in dd/mm/yyyy text format | 1,664 | 0 | Parsed with explicit format; 0 unparseable remain |
| transactions | Missing payment_method | 2,496 | 323 | Recovered from other lines in same basket; rest labelled 'Unknown' |
| transactions | Missing customer_id | 2,496 | 331 | Recovered from same basket; the rest are kept as unidentified shoppers |
| transactions | Negative quantity | 166 | 0 | Converted to absolute value, flagged |
| transactions | Quantity outliers | 556 | 0 | Replaced with group median (wholesale fence = 38, others = 10), flagged |
| transactions | Missing price | 832 | 0 | Imputed with median price of same product, month and promo state; flagged |
| customers | Missing county | 30 | 0 | Inferred from county of most-visited store |

## Check against the generator's log

Issues planted by `generate_data.py` versus what this pipeline found.

| Issue | Planted | Found |
|---|---:|---:|
| Duplicate rows (transactions) | 832 | 832 |
| Quantity outliers | 499 | 556 |
| Negative quantity | 166 | 166 |
| Dates in dd/mm/yyyy | 1,664 | 1,664 |
| Missing price | 832 | 832 |

## Features added

`revenue`, `year`, `month`, `week`, `day_of_week`, `is_weekend`, `is_payday_period`, `is_public_holiday`, store county/region, product category; customer-level recency, frequency, monetary, average basket value, tenure, promo share, M-Pesa share, stores visited.

## Flags kept for transparency

`qty_negative_fixed`, `qty_outlier_fixed`, `price_imputed`, `customer_known` mark rows that were repaired, so later analysis can include or exclude them.