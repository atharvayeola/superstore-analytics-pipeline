# Superstore Analytics Pipeline

An end-to-end **data engineering** → **analytics** project on the Sample Superstore dataset.  
This pipeline ingests raw sales data, cleans and enriches it, builds a dimensional warehouse, and serves an interactive Dash dashboard.

---

## 🚀 Project Overview

1. **Stage 1 (Bronze)**: Ingest raw CSV → clean & standardize → save as **`bronze_superstore.parquet`**  
2. **Stage 2 (Silver)**: Derive business metrics (profit margin, monthly aggregates, top customers) → save as **`silver/*.parquet`**  
3. **Stage 3 (Gold)**: Build star schema (dimensions + fact tables) → load into **`gold_superstore.db`** (SQLite)  
4. **Stage 4 (Dashboard)**: Query warehouse → render interactive charts in **Dash** (`app.py`)

---

## 📦 Dataset

- **Source CSV**: `superstore.csv` (download and place in root)  
**Original dataset**: [Sample Superstore dataset (Kaggle)](https://www.kaggle.com/datasets/vivek468/superstore-dataset-final)  
  - Columns include:  
    `Order ID`, `Order Date`, `Ship Date`, `Customer ID`, `Category`, `Sales`, `Profit`, etc.

---

## 📁 Repository Structure

![image](https://github.com/user-attachments/assets/7f1475f6-e04e-4c96-a403-dadf0766cfcd)


---

## ⚙️ Prerequisites

- Python 3.7+  
- (Optional) A virtual environment  
- Install dependencies:
  ```bash
  pip install -r requirements.txt

## 🛠️ Usage

### 1. Stage 1 – Bronze (Ingestion & Cleaning)
```bash
python stage1_bronze.py
```
- Reads `superstore.csv`  
- Drops duplicates & invalid rows  
- Parses dates & trims strings  
- Derives `Order_Month`, `Order_Year`  
- Writes **`bronze_superstore.parquet`**

### 2. Stage 2 – Silver (Feature Engineering & Aggregates)
```bash
python stage2_silver.py
```
- Reads `bronze_superstore.parquet`  
- Calculates `Profit_Margin`  
- Aggregates:
  - Category metrics → `silver/category_metrics.parquet`  
  - Region metrics   → `silver/region_metrics.parquet`  
  - Monthly metrics  → `silver/monthly_metrics.parquet`  
  - Top customers    → `silver/customer_metrics.parquet`

### 3. Stage 3 – Gold (Warehouse Load)
```bash
python stage3_gold.py
```
- Reads silver Parquet files  
- Builds dimensions (`dim_category`, `dim_region`, `dim_date`)  
- Builds fact tables (`fact_category`, `fact_region`, `fact_monthly`, `fact_customer`)  
- Loads everything into **`gold_superstore.db`** (SQLite)

### 4. Stage 4 – Dashboard (Dash App)
```bash
python app.py
```
- Connects to `gold_superstore.db`  
- Renders:
  - Monthly Sales & Profit Trend  
  - Sales by Category & Region  
  - Top 10 Customers by Sales  
- View at: `http://127.0.0.1:8050/`
