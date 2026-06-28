
## 1. Project Overview
This project focuses on building an audit-ready, standardized data-engineering pipeline to process, clean, and validate order-level retail transaction logs. The source dataset was consolidated from multiple legacy internal architectures, e-commerce web storefronts, and regional Point-of-Sale (POS) terminals. 

The primary objective was to resolve widespread data degradation issues—including structural string corruption, conflicting database records, date formatting discrepancies, out-of-bounds promotional discounts, and financial calculation mismatches—transforming the raw data into an analysis-ready database container while documenting every technical transformation.

## 2. Problem Summary
The initial unstructured multi-system transaction export suffered from a high rate of data quality errors that compromised the accuracy of business reporting and financial audits. Key data issues included:
* **Structural String Gaps:** Truncated data parameters leading to missing attributes across geographic tracking and logistics provider fields.
* **Chronological Discrepancies:** Inconsistent and mixed formatting styles string-mapping time parameters where carrier delivery events appeared to occur before order generation.
* **Financial Equation Deviations:** Systemic accounting discrepancies between ledger entries for transactional revenue, unit breakdowns, and profit margins.
* **Redundant Ingestion Rows:** Cross-system record duplication inflating top-line operational metrics.

## 3. Dataset Description
The database environment consists of **932 raw transaction lines** defined across a baseline schema of **21 columns**:

| Col # | Field Identifier | Database Type | Operational System Mapping Description |
| :--- | :--- | :--- | :--- |
| 1 | `order_id` | Alphanumeric | Unique system transactional key constraint |
| 2 | `order_date` | Text / Timestamp | Target timestamp for point-of-sale customer checkout |
| 3 | `ship_date` | Text / Timestamp | Logistical carrier departure handoff event timestamp |
| 4 | `customer_id` | Alphanumeric | Unique tracking reference number assigned to buyers |
| 5 | `customer_name` | Text String | Full individual name record of the consumer |
| 6 | `segment` | Categorical | Business market sector grouping classification |
| 7 | `region` | Categorical | Regional administrative center tracking cell |
| 8 | `state` | Categorical | State level geographic classification assignment |
| 9 | `city` | Categorical | Specific municipal market node territory |
| 10 | `category` | Categorical | Broad top-level inventory group division label |
| 11 | `sub_category` | Categorical | Fine-grained classification tier label for specific stock lines |
| 12 | `product_name` | Text String | Full alphanumeric title identifier text description |
| 13 | `ship_mode` | Categorical | Selected priority carrier tier logistics option |
| 14 | `quantity` | Integer | Absolute unit volume capacity transacted |
| 15 | `unit_price` | Float / Currency | Base list retail price currency value per unit |
| 16 | `discount` | Float / Percent | Fractional promotion discount markdown index value |
| 17 | `sales` | Float / Currency | Top-line transaction total captured by the POS ledger |
| 18 | `cost` | Float / Currency | Internal manufacturing, logistics, or wholesale overhead cost |
| 19 | `profit` | Float / Currency | Reported net profit value captured by accounting |
| 20 | `payment_status`| Categorical | Financial clearance authorization condition status |
| 21 | `order_status`  | Categorical | Administrative status of fulfillment pipeline execution |

---

## 4. Tools Used
The operational processing framework was built using **Microsoft Excel** and audited with **Python (Pandas/NumPy)** to ensure full compliance with financial verification and data-cleansing standards:
* **Data Transformation Functions:** Applied nested logic equations—such as `=PROPER(TRIM(CLEAN(cell)))`—to standardize text cells. Used `=IF()`, `=IFERROR()`, `=ABS()`, and conditional date parsing expressions to rebuild broken system entries.
* **Data Auditing Utilities:** Used specialized spreadsheet management tools like *Deduplication Arrays, Conditional Formatting Rules, and Data Validation Lists* to isolate out-of-bounds parameters.
* **Aggregations & Analytics Engine:** Generated structural cross-tab tables using *Dynamic Excel Tables and Pivot Engine Matrix Tools* to group multi-index analytical targets.


## 5. Cleaning Steps Performed

### Step 1: Text Standardization & Trim Operations
Applied vector string formatting to fields containing leading, trailing, or irregular non-breaking whitespace variations. The pipeline used clean case conversion logic (`PROPER`, `TRIM`) across fields including `customer_name`, `segment`, `region`, `state`, `city`, `category`, `sub_category`, `ship_mode`, `payment_status`, and `order_status`. This step resolved character mismatches (e.g., standardizing entries like `"  Cancelled "`, `"completed"`, and `"COMPLETED"`).

### Step 2: Multi-Format Date Ingestion Parsing
Normalized conflicting temporal formats—including raw text varieties like *“21 Jul 2024”*, *“08/31/2024”*, and *“2024-02-20”*—into standard `YYYY-MM-DD` data types. Added a chronological loop to correct short-format date inversion bugs where system day/month swaps caused ship events to display before purchase events.

### Step 3: Deduplication Framework Processing
Divided duplicate tracking records into two functional queues:
* **Exact Copy Deletion:** Evaluated entries where all data values overlapped, removing **20 rows** to prevent revenue inflation.
* **Conflicting Row Identification:** Retained and isolated entries sharing an `order_id` but containing differing parameters (**31 rows**), adding data-quality flags to ensure ledger transparency.

### Step 4: Re-Calculation & Balance Sheet Engineering
Audited financial logic anomalies across every row. Whenever transaction data points deviated by more than a **$1.00 tolerance margin**, values were programmatically recalculated using standard corporate math constraints:
$$\text{Calculated Sales} = \text{Quantity} \times \text{Unit Price} \times (1 - \text{Discount})$$
$$\text{Calculated Profit} = \text{Calculated Sales} - \text{Cost}$$


## 6. Business Rules Applied

* **Missing Region Parameters:** Filled empty geographic attributes with the fallback string `"Unknown"` and logged the updates for tracking (**26 instances**).
* **Missing Shipping Method Parameters:** Imputed blank shipping fields as `"Unknown"` to maintain structural integrity across tracking reports (**22 instances**).
* **Missing Promotion Parameters:** Set missing values to `0.00` if financial fields matched calculated targets; otherwise, flagged the entries for auditing (**18 instances**).
* **Invalid Discount Parameters:** Marked sub-zero discount entries ($< 0\%$) (**16 instances**) and extreme anomalies exceeding the corporate maximum threshold of $70\%$ (**8 instances**) as invalid promotion codes.
* **Pipelines Exclusions Rule:** Filtered out transactions flagged as **Cancelled Orders** (**146 instances**) or **Failed Payments** (**69 instances**) from standard sales performance summaries to safeguard gross revenue projections from inflation.
* **Isolated Reverse-Logistics Summary:** Isolated **Refunded Orders** (**72 instances**) into a separate ledger tracking sheet to evaluate chargeback liabilities without skewing performance baselines.


## 7. Summary of Data Quality Issues Found
The automated parsing run mapped the following distribution of data quality anomalies within the source entry log:
