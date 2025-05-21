# 📓 Notebooks: Step-by-Step RFM Analysis

## 📒 01_EDA_and_Cleaning.ipynb

### 1.1 Introduction & Business Context  
- **Goal**: Understand raw sales data structure, spot anomalies, and plan cleaning steps to support accurate RFM segmentation.  
- **Chain of thought**: “Before jumping into scoring, I need to ensure every record is valid—missing or junk values will skew recency, frequency and monetary metrics.”

### 1.2 Load & Preview Data  
- `load_data()`: imported raw CSV, parsed dates, set display options.  
- **Why**: Quickly verify row count, data types, and glimpse sample records to form initial hypotheses (“Is InvoiceDate consistently formatted? Are there unexpected zeroes?”).

### 1.3 Data Quality Assessment  
- **Functions**:  
  - `assess_missing_values(df)` → % missing per column  
  - `assess_duplicates(df)` → count of duplicate InvoiceNo × CustomerID  
- **Findings**:  
  - 10% CustomerID missing → must decide guest-ID strategy  
  - 5% ShippingCost missing → median imputation possible  
  - 7% WarehouseLocation missing → mode imputation appropriate  
- **Chain of thought**: “These three fields are critical: recency uses date, monetary uses shipping‐inclusive price, and geographic analysis uses warehouse. I can’t ignore them.”

### 1.4 Missing-Value Treatments  
- **Strategies**:  
  1. **CustomerID**: generate synthetic IDs starting from 90000 to preserve unique “guest” behavior  
  2. **ShippingCost**: fill by country‐level median to reflect localized shipping patterns  
  3. **WarehouseLocation**: fill missing with global mode (most common location)  
- **Before / After**: Show bar charts & tables of missing % pre‐ vs post‐imputation.  
- **Chain of thought**: “Each imputation choice preserves real variation while avoiding misleading ‘zero’ or dropcount.”

### 1.5 Data Model & Star Schema Preview  
- **ER Diagram**: fact_sales ← dim_customer, dim_product, dim_shipment, dim_date  
- **Rationale**:  
  - Isolate dimensional attributes for faster aggregations  
  - Simplify eventual Tableau data extracts  
- **Code snippets**: simple pandas → CSV exports per table  
- **Chain of thought**: “I know Tableau performs best on flattened starized extracts—preparing dimensions now speeds up later viz.”

### 1.6 Save Processed Data  
- Exports:  
  - `/data/processed/sales_fact.csv`  
  - `/data/processed/dim_*.csv`  
- **Next step**: Move on to scoring RFM across cleaned fact_sales.

---

---

## 📒 02_RFM_Scoring_and_Segmentation.ipynb

### 2.1 Introduction & Objectives  
- **Goal**: Compute Recency, Frequency, Monetary scores (1–4 each), combine into RFM, then map to customer segments.  
- **Chain of thought**: “Segment definitions drive marketing offers—thresholds must reflect underlying distribution, not arbitrary cutoffs.”

### 2.2 Recency Analysis  
- Compute “days_since_last_purchase” per CustomerID using max(`InvoiceDate`).  
- **Visualize**: histogram + boxplot to inspect skewness.  
- **Thresholds**: 30, 90, 180 days → assign Recency score (4 = most recent).  
- **Chain of thought**: “I want fine granularity for customers who shopped in the last month vs last quarter.”

### 2.3 Frequency Analysis  
- Compute total invoice count per CustomerID.  
- **Visualize**: bar / density plot of purchase counts.  
- **Quantile breakpoints**: top 25% = score 4, next 25% = 3, etc.  
- **Chain of thought**: “Uniform bins by quantile ensure balanced group sizes for frequency.”

### 2.4 Monetary Analysis  
- Sum `UnitPrice*Quantity + ShippingCost` per CustomerID.  
- **Visualize**: distribution & log‐scale view.  
- **Quartile scoring**: mirror frequency approach.  
- **Chain of thought**: “Outliers exist—log transform helps choose realistic cutpoints.”

### 2.5 Compute Composite RFM Score  
```python
rfm['RFM_Score'] = rfm.Recency + rfm.Frequency + rfm.Monetary




| Score Range | Segment Name      |
| ----------- | ----------------- |
| 10 – 12     | Champions         |
| 7 – 9       | Loyal Customers   |
| 6           | Regular Customers |
| 4 – 5       | At Risk           |
| ≤ 3         | Lost Customers    |



