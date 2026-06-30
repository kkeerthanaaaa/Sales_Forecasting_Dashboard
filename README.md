# AI-Powered Sales Forecasting Dashboard

## Overview

This project delivers an end-to-end **Sales Forecasting Dashboard** built with **Power BI**, powered by a **Facebook Prophet** time-series forecasting model trained in Python (Google Colab). The dashboard provides actionable insights into historical sales performance and future sales predictions across multiple datasets.

---

## Dashboard Preview

The dashboard includes the following visualizations and KPIs:
</br></br>
<img width="1573" height="887" alt="Screenshot 2025-12-11 173109" src="https://github.com/user-attachments/assets/3cc543ed-c8fa-4aed-a1f9-9e8adb9e9aee" />
</br>

| Section | Description |
|---|---|
| **Actual vs Predicted Sales** | Area chart comparing historical sales to Prophet model predictions over time |
| **Total Forecast Future** | KPI card showing total forecasted sales value |
| **Next 30 Days Forecast** | Near-term sales projection |
| **Sum of Actual Sales** | Aggregate of all historical sales (~6bn) |
| **High/Low Season Months** | Identifies peak (Jul 2014) and low (Oct 2016) sales periods |
| **Sales by Category** | Slicer for Furniture, Office Supplies, Technology |
| **Sales by Region** | Filter by Central, South, East, West |
| **Sales by Year** | Bar chart of annual sales totals |
| **Sales by Month** | Line chart showing seasonal monthly patterns |

---

## Tech Stack

| Tool | Purpose |
|---|---|
| **Python** | Data preprocessing & forecasting model |
| **Facebook Prophet** | Time-series forecasting |
| **Pandas / NumPy** | Data manipulation |
| **Google Colab** | Model training environment |
| **Power BI** | Dashboard visualization |

---

## Data Sources

The following CSV files were used as inputs:

| File | Description |
|---|---|
| `mock_kaggle.csv` | Synthetic sales data (`data`, `venda`, `estoque`, `preco` columns) |
| `store.csv` | Store metadata (competition distance, promotions, store type) |
| `train.csv` | Rossmann store sales training data |
| `test.csv` | Rossmann store sales test data |
| `Sample - Superstore.csv` | Superstore sales data with category, region, profit |

---

## How It Works

### Step 1 — Data Preparation (Python / Google Colab)
- All CSV files are loaded and merged into a unified dataframe
- Date columns (`Date`, `Order Date`, `data`) are normalized into a single `date_final` column
- Sales columns (`Sales`, `venda`) are consolidated into `sales_final`
- Data is aggregated to daily totals and formatted for Prophet (`ds`, `y`)

### Step 2 — Forecasting with Prophet
- Model is trained on historical daily sales (2013–2016)
- Forecast is generated for **90 days** beyond the last known date
- Output includes `predicted_sales`, `lower_bound`, `upper_bound`, and `horizon_type` (fitted vs forecast)
- Results exported to `sales_forecast_for_powerbi.csv`

### Step 3 — Power BI Dashboard
- The exported CSV is imported into Power BI
- KPI cards, time-series charts, and slicers are configured
- Filters for **Category** and **Region** enable interactive exploration

---

## Model Performance

Evaluated on the last 60 days of historical data:

| Metric | Value |
|---|---|
| **MAE** | 1,888,283 |
| **RMSE** | 2,757,246 |
| **MAPE** | ~1,330,519% |

> ⚠️ **Note:** The very high MAPE is due to days with near-zero actual sales in the test set (division by near-zero inflates the percentage error). The model captures overall trends well, but the mixed multi-source dataset (with very different sales scales) introduces noise. Consider training separate models per data source for production use.

---

## Key Findings

- **Peak sales period:** July 2014
- **Lowest sales period:** October 2016
- The Prophet model identified clear **weekly and yearly seasonality** in the data
- The forecast shows a **declining trend** post-mid-2015, reflecting the data distribution across the merged sources

---

## Project Structure

```
sales-forecasting-dashboard/
│
├── data/
│   ├── mock_kaggle.csv
│   ├── store.csv
│   ├── train.csv
│   ├── test.csv
│   └── Sample - Superstore.csv
│
├── notebooks/
│   └── Sales_Forecasting_Dashboard.ipynb   # Google Colab notebook
│
├── output/
│   └── sales_forecast_for_powerbi.csv      # Prophet forecast output
│
├── powerbi/
│   └── SalesForecastingDashboard.pbix      # Power BI dashboard file
│
└── README.md
```

---

## Getting Started

### Prerequisites
- Python 3.8+
- Google Colab (or local Jupyter)
- Power BI Desktop

### Run the Forecasting Model

1. Open `Sales_Forecasting_Dashboard.ipynb` in Google Colab
2. Upload the data files when prompted
3. Run all cells — the notebook will:
   - Preprocess and merge the data
   - Train the Prophet model
   - Export `sales_forecast_for_powerbi.csv`
4. Download the output CSV

### Open the Power BI Dashboard

1. Open `SalesForecastingDashboard.pbix` in Power BI Desktop
2. Update the data source path to point to your `sales_forecast_for_powerbi.csv`
3. Refresh the data
4. Explore the dashboard using the Category and Region slicers

---

## Future Improvements

- Train separate Prophet models per store/category for better accuracy
- Add confidence interval bands to the Power BI chart
- Incorporate external regressors (promotions, holidays) into the Prophet model
- Schedule automated retraining via Azure ML or similar
- Add drill-through pages for store-level and product-level analysis

---

## Author

Built as a sales analytics and forecasting portfolio project using open datasets.

---

## License

This project is for educational and portfolio purposes.
