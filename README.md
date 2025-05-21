# 🚀 Customer Segementation Using RFM(Recency, Frequency, Monetary) 

A complete end-to-end customer segmentation case study using RFM analysis, from raw data cleaning and star-schema design to Python automation and interactive Tableau dashboards.

---

## 🔍 Overview

- **Business Goal**: Identify high-value and at-risk customers to boost retention and revenue.  
- **Data**: 38 k customers, €43 M sales, multiple European markets.  
- **Approach**:  
  1. **EDA & Cleaning** – handle missing CustomerIDs, shipping costs, warehouse locations  
  2. **Star Schema** – build fact & dimension tables for fast analytics  
  3. **RFM Scoring** – recency, frequency, monetary quantiles → composite score  
  4. **Segmentation** – Champions, Loyal, Regular, At-Risk, Lost  
  5. **Visualization** – dynamic Tableau dashboard + key screenshots  
  6. **Business Impact** – +20 % Q4 performance, targeted retention campaigns

---

## 📁 Repo Structure



├── notebooks/ # 01_EDA_and_Cleaning.ipynb
│ # 02_RFM_Scoring_and_Segmentation.ipynb
├── data/ # raw → processed → final CSVs
├── tableau/ # Retail_RFM_Dashboard.twbx
├── screenshots/ # annotated dashboard PNGs
├── presentation/ # Case_Study.pptx / .pdf
└── README.md # This file



### 🎯 Results & Next Steps
- Key Finding: 0.6 % of customers (“Champions”) drive 9 % of revenue; 44 % are “Lost” yet contribute only 14 %.
- Action: Tailored campaigns for “Regular” → “Loyal” uplift; churn-risk mitigation for “At-Risk.”
- Future: Predictive churn modeling, real-time segmentation updates, automated reporting.

