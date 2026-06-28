# Task 5: Apply Business Rules

## Objective

To ensure that the retail sales dataset is reliable and suitable for business analysis, a series of business validation rules are applied. These rules standardize missing values, identify invalid records, and ensure that sales summaries include only legitimate completed transactions.

---

# Business Rule Summary

| Rule Area                        | Business Rule Applied                                                                                        | Action Taken                                                                                                                                              |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Missing Region**               | Missing region values should not remain blank.                                                               | Missing values are replaced with **"Unknown"** and each affected record is flagged in the data quality report.                                          |
| **Missing Ship Mode**            | Shipping method is required for logistics analysis.                                                          | Missing values are replaced with **"Unknown"** and flagged for review.                                                                                   |
| **Missing Discount**             | If all other sales fields are valid, missing discounts are treated as zero.                                | Missing discount values are replaced with **0** only when quantity, unit price, and sales values are valid. Otherwise, records are flagged for review. |
| **Negative Discount**            | Discounts cannot be negative.                                                                                | Records containing negative discount values are flagged as **Invalid Discount**. No automatic correction is performed.                                  |
| **Discount Above Allowed Range** | Discounts exceeding the permitted business limit (greater than 100% or the specified threshold) are invalid. | Records are flagged as **Invalid Discount** and excluded from analytical summaries until reviewed.                                                       |
| **Cancelled Orders**             | Cancelled orders should not contribute to completed sales reporting.                                         | Cancelled orders are retained in the cleaned dataset but excluded from completed sales summaries.                                                        |
| **Failed Payments**              | Orders with failed payments should not be recognized as completed sales.                                     | Failed payment records are excluded from completed sales calculations while remaining available for operational reporting.                               |
| **Refunded Orders**              | Refunded transactions should be reported separately.                                                         | Refunded orders are excluded from completed sales totals and summarized independently for financial review.                                              |
| **Ship Date Before Order Date**  | Shipping cannot occur before an order is placed.                                                             | Records where the ship date preceded the order date are flagged as **Invalid Shipping Record** for manual investigation.                                 |

---

# Business Rule Decisions

## 1. Missing Region

Blank region values wvere replaced with **"Unknown"** to preserve record completeness while clearly identifying observations requiring further validation.

---

## 2. Missing Ship Mode

Missing shipping methods are standardized as **"Unknown"**, allowing the records to remain available for analysis without introducing inaccurate shipping classifications.

---

## 3. Missing Discount

Where quantity, unit price, and sales values are internally consistent, a missing discount is interpreted as **0%**, assuming no discount had been applied.

Records with additional inconsistencies are not modified automatically and are flagged for review.

---

## 4. Invalid Discount Values

Discount values below zero or above the permitted business range violate company pricing rules.

These records are **not corrected automatically** because the true discount could not be determined from the available information.

Instead, they are clearly flagged for manual verification.

---

## 5. Cancelled Orders

Cancelled orders do not represent completed revenue.

These records remain in the cleaned dataset for operational reporting but are excluded from completed sales summaries to avoid overstating business performance.

---

## 6. Failed Payments

Orders with failed payments do not generate realized revenue.

Accordingly, these transactions are excluded from completed sales reporting while remaining available for operational analysis.

---

## 7. Refunded Orders

Refunded transactions are summarized separately to support financial reconciliation and customer service reporting.

Although they remain valid operational records, they are excluded from completed sales calculations.

---

## 8. Shipping Date Validation

Records where the ship date occurred before the order date violate normal business operations.

These observations are retained but flagged as **Invalid Shipping Records** for manual investigation.

---

# Overall Business Impact

Applying these business rules improves the accuracy and reliability of the dataset by:

* Standardizing missing categorical values.
* Correctly handling missing discounts.
* Identifying invalid pricing records.
* Preventing cancelled and failed-payment orders from inflating sales performance.
* Separating refunded transactions for financial reporting.
* Detecting invalid shipping timelines.

These validation steps ensure that the final dataset is suitable for downstream reporting, dashboard development, and business analysis while preserving questionable records for further review rather than deleting them automatically.
